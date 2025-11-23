---
title: 'File Handling in Python'
date: 2018-07-23
permalink: /posts/2018/07/file-handling-in-python/
tags:
  - Python
  - File Handling
  - I/O Operations
  - Programming
---

Python has convenient built-ins to work with files. The intentions of this post is to discuss on various modes of open() and see them through examples. open() is a built-in function that returns a file object, also called a handle, as it is used to read or modify the file accordingly. We will start by opening file with default parameters and see through examples, the important modes of file reading and writing and also see the parameters of the open() built-in.

## Using open() with default parameters.
```python
>>> file_handle = open('existingfile.txt')
>>> type(file_handle)
<class '_io.TextIOWrapper'>
>>>
>>> file_handle2 = open('nonexistentfile.txt')
Traceback (most recent call last):
  File "", line 1, in 
FileNotFoundError: [Errno 2] No such file or directory: 'nonexistentfile.txt'
>>>
```

The open() built-in has one required parameter, file. file is either a text or byte string giving the path of the file to be opened or an integer file descriptor of the file to be wrapped. (If a file descriptor is given, it is closed when the returned I/O object is closed, unless closefd is set to False.) By default, the file is opened in read text mode. If the file to be read isn't present in the specified path, a FileNotFoundError is raised.

## mode (Different file modes).
A file can be opened for reading purpose and writing purpose. This can be specified through the optional argument mode of the open() built-in. mode is a string that specifies the mode in which the file is opened. As we have seen in the example above, it defaults to 'r' i.e for reading in text mode. Another common value for mode is 'w' for writing. The file is truncated if it already exists while opened in 'w' mode. 'x' for creating and writing to a new file, and 'a' for appending (which on some Unix systems, means that all writes append to the end of the file regardless of the current seek position). Following are the available modes:

| Mode | Meaning |
|------|---------|
| 'r' | open for reading(default) |
| 'w' | open for writing, truncating the file first |
| 'x' | create a new file and open it for writing |
| 'a' | open for writing, appending to the end of the file if it exists |
| 'b' | binary mode |
| 't' | text mode(default) |
| '+' | open a disk file for updating (reading and writing) |

The default mode is 'rt' (open for reading text). The 'x' mode implies 'w' and raises an `FileExistsError` if the file already exists.

Python distinguishes between files opened in binary and text modes, even when the underlying operating system doesn't. Files opened in binary mode (appending 'b' to the mode argument) return contents as bytes objects without any decoding. In text mode (the default, or when 't' is appended to the mode argument), the contents of the file are returned as strings, the bytes having been first decoded using a platform-dependent encoding or using the specified encoding if given.

## Writing contents to a file:
```python
>>> file_handler = open("text.txt", "w") # or use "wt"
>>> file_handler
<_io.TextIOWrapper name='text.txt' mode='w' encoding='UTF-8'>
>>> file_handler.write("This will use the default encoding from the machine.")
>>> file_handler.close()
```

In the above code snippet, we open the file in write text mode and do not specify the encoding. When encoding not specified, it defaults to platform specific encoding. Also note that encoding argument should be used for text mode only.

Let's try to read the file that we wrote and let's see what happens when we pass a different encoding than the one used to write.

```python
>>> file_handler = open("text.txt", mode="r", encoding="UTF-16")
>>> file_handler
<_io.TextIOWrapper name='writetext.txt' mode='r' encoding='UTF-16'>
>>> contents = file_handler.read()
Traceback (most recent call last):
  File "", line 1, in 
  File "/usr/lib/python3.6/codecs.py", line 321, in decode
    (result, consumed) = self._buffer_decode(data, self.errors, final)
  File "/usr/lib/python3.6/encodings/utf_16.py", line 67, in _buffer_decode
    raise UnicodeError("UTF-16 stream does not start with BOM")
UnicodeError: UTF-16 stream does not start with BOM
>>>
```

Therefore it is important to use the same encoding to read the file as it was when written.

```python
>>> file_handler = open('text.txt', 'r', encoding='UTF-8')
>>>
>>> file_handler
<_io.TextIOWrapper name='text.txt' mode='r' encoding='UTF-8'>
>>>
>>> contents = file_handler.read()
>>> contents
'This will use the default encoding from the machine.'
>>>
```

## Writing in binary mode:
"Binary" files are any files where the format isn't made up of readable characters. Binary files can range from image files like JPEGs or GIFs, audio files like MP3s or binary document formats like Word or PDF.

```python
>>> file_handler = open('text.txt', 'wb')
>>> file_handler
<_io.BufferedWriter name='text.txt'>
>>>
>>> byte_arr = [120, 3, 255, 0, 100]
>>> binary_format = bytearray(byte_arr)
>>> file_handler.write(binary_format)
>>> file_handler.close()
```

## Reading in binary mode:
```python
>>> file_handler = open('text.txt', 'rb')
>>>
>>> file_handler
<_io.BufferedReader name='text.txt'>
>>> contents = file_handler.read()
```

## Parameters of open() built-in:

| Parameter | Parameter Type | Default value | Description |
|-----------|----------------|---------------|-------------|
| file | Required |  | The path to the file. |
| mode | Optional | 'r' | The mode to open the file in. |
| buffering | Optional | -1 | buffering is an optional integer used to set the buffering policy. Pass 0 to switch buffering off (only allowed in binary mode), 1 to select line buffering (only usable in text mode), and an integer > 1 to indicate the size of a fixed-size chunk buffer. When no buffering argument is given, the default buffering policy works as follows: Binary files are buffered in fixed-size chunks; the size of the buffer is chosen using a heuristic trying to determine the underlying device's "block size" and falling back on `io.DEFAULT_BUFFER_SIZE`. On many systems, the buffer will typically be 4096 or 8192 bytes long. "Interactive" text files (files for which isatty() returns True) use line buffering. Other text files use the policy described above for binary files. |
| encoding | Optional | None | encoding is the name of the encoding used to decode or encode the file. This should only be used in text mode. The default encoding is platform dependent, but any encoding supported by Python can be passed. See the codecs module for the list of supported encodings. |
| errors | Optional | None | errors is an optional string that specifies how encoding errors are to be handled—this argument should not be used in binary mode. Pass 'strict' to raise a ValueError exception if there is an encoding error (the default of None has the same effect), or pass 'ignore' to ignore errors. (Note that ignoring encoding errors can lead to data loss.) See the documentation for codecs.register or run 'help(codecs.Codec)' for a list of the permitted encoding error strings. |
| newline | Optional | None | newline controls how universal newlines works (it only applies to text mode). It can be None, '', '\n', '\r', and '\r\n'. It works as follows: On input, if newline is None, universal newlines mode is enabled. Lines in the input can end in '\n', '\r', or '\r\n', and these are translated into '\n' before being returned to the caller. If it is '', universal newline mode is enabled, but line endings are returned to the caller untranslated. If it has any of the other legal values, input lines are only terminated by the given string, and the line ending is returned to the caller untranslated. On output, if newline is None, any '\n' characters written are translated to the system default line separator, os.linesep. If newline is '' or '\n', no translation takes place. If newline is any of the other legal values, any '\n' characters written are translated to the given string. |
| closefd | Optional | True | If closefd is False, the underlying file descriptor will be kept open when the file is closed. This does not work when a file name is given and must be True in that case. |
| opener | Optional | None | A custom opener can be used by passing a callable as *opener*. The underlying file descriptor for the file object is then obtained by calling *opener* with (*file*, *flags*). *opener* must return an open file descriptor (passing os.open as *opener* results in functionality similar to passing None). |