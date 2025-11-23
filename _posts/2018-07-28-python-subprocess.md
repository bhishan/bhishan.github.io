---
title: 'Python Subprocess'
date: 2018-07-28
permalink: /posts/2018/07/python-subprocess/
tags:
  - Python
  - Subprocess
  - Process Management
  - System Administration
---

Through this post, we will discuss and see via examples the purpose of subprocess, how to spawn processes, how to connect to their input/output and error pipes, etc.

## subprocess
As the name suggests, subprocess is used to spawn sub-processes. It also allows for us to get the output from the process, error if any as well as ability to send keystrokes to the input of the process which generally means we can communicate with various processes. Subprocess was introduced to replace the need for os.system and os.spawn*

## subprocess.run()
The subprocess.run() was added from Python3.5 and comes with all other higher versions.
It is the recommended method to invoke a subprocess for all use cases that it can handle. We can use Popen for more advanced use cases.

```python
>>> import subprocess
>>> ls_process = subprocess.run(['ls', '-l'])
>>> ls_process
CompletedProcess(args=['ls', '-l'], returncode=0)
>>> ls_process.args
['ls', '-l']
>>> ls_process.returncode
0
>>> ls_process.stdout
>>> ls_process.stderr
>>>
```

The subprocess.run() returns a CompletedProcess instance which has attributes args, retuncode, stdout and stderr. The stdout and stderr attributes of the CompletedProcess instance will hold None unless explicitly specified on subprocess.run() call to capture them.

## Capture standard output and error
```python
>>> ls_process = subprocess.run(['ls'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
>>> ls_process
CompletedProcess(args=['ls'], returncode=0, stdout=b'networkenv\ntest.py\n', stderr=b'')
>>> ls_process.stdout
b'networkenv\ntest.py\n'
>>>
>>> ls_process.stderr
b''
>>>
```

There is an optional argument "input", allowing you to pass a string to the subprocess's stdin. If you use this argument you may not also use the Popen constructor's "stdin" argument, as it will be used internally.

## Another useful argument can be timeout
```python
>>> subprocess.run(['ls'], timeout=0.000000000000000000000000000000000000001)

subprocess.TimeoutExpired: Command '['ls']' timed out after 1e-39 seconds
```

When timeout argument is passed to the run(), it raises subprocess.TimeoutExpired error. if the process failed to complete in the given time.

## check is another argument
check is another argument that can be passed which raises CalledProcessError exceptions when the exit code of the subprocess was non-zero. A zero exit code signifies success. The CalledProcessError object will hold the return code in the returncode attribute. You can catch this error and based on the returncode, perform operations as per your needs.

```python
>>> ls_process = subprocess.run("exit 1", shell=True, check=True)
Traceback (most recent call last):
  File "", line 1, in 
  File "/usr/lib/python3.6/subprocess.py", line 418, in run
    output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command 'exit 1' returned non-zero exit status 1.
>>>
```

## Popen Constructor
The underlying process creation and management of the module is handled by the Popen class. It offers a lot of flexibility so that developers are able to handle the less common cases not covered by the convenience function subprocess.run(). The Popen() is used to execute a child program in a new process. On POSIX, the class uses os.execvp()-like behavior to execute the child program. On Windows, the class uses the Windows CreateProcess() function. The underlying examples are just for the purpose of showing usage of Popen and doesn't necessarily mean subprocess.run() cannot achieve these operations.

## Example Usage of Popen
```python
>>> import subprocess
>>>
>>> wc_process = subprocess.Popen(['wc', '-l', 'test.py'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
>>>
>>> echo_process

>>> stdout, stderr = wc_process.communicate()
>>> stdout
b'35 test.py\n'
>>> stderr
b''
>>>
```

In the above example we are using Unix tool wc (word count) and also specifying to PIPE the standard output and error.

## Direct Stdout and Stderr to file
```python
>>> echo_process = subprocess.Popen(['wc', '-l', 'test.py'], stdout=open('out.txt', 'w'), stderr=open('error.txt', 'w'))
>>> echo_process

>>>
>>> with open('out.txt', 'r') as outbuff:
...     print(outbuff.read())
...
35 test.py

>>>
```

## Passing input to stdin
```python
>>> script_process = subprocess.Popen(['python3', 'expect_input.py'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, encoding='UTF-8')
>>>
>>> script_process.communicate("Bhishan")
('Enter your nameBhishan\n', '')
>>>
```

I have a helper script named expect_input.py which expects input and prints it. You can pass input to stdin via communicate() method.

## PIPE output of one subprocess as input to another
```python
>>> ls_process = subprocess.Popen('ls', stdout=subprocess.PIPE)
>>> grep_process = subprocess.Popen(['grep', '.py'], stdin=ls_process.stdout, stdout=subprocess.PIPE)
>>> stdout, stderr = grep_process.communicate()
>>> stdout
b'expect_input.py\ntest.py\n'
>>>
```

In the above code snippet we passed the ls_process.stdout to the stdin of the grep process. The ls_process lists the current working directory files and the later subprocess named grep_process takes the result from ls_process as it's input and lists the ones with py in the names.

## Other parameters of Popen

| Argument | Description | Default |
|----------|-------------|---------|
| args | A string or a sequence of program arguments. eg : "ls", ["ls", "-l"] | |
| bufsize | bufsize will be supplied as the corresponding argument to the open() function when creating the stdin/stdout/stderr pipe file objects: 0 means unbuffered (read and write are one system call and can return short) 1 means line buffered (only usable if universal_newlines=True i.e., in a text mode) any other positive value means use a buffer of approximately that size negative bufsize (the default) means the system default of io.DEFAULT_BUFFER_SIZE will be used. | -1 |
| executable | The executable argument specifies a replacement program to execute. It is very seldom needed. When shell=False, executable replaces the program to execute specified by args. However, the original args is still passed to the program. Most programs treat the program specified by args as the command name, which can then be different from the program actually executed. On POSIX, the args name becomes the display name for the executable in utilities such as ps. If shell=True, on POSIX the executable argument specifies a replacement shell for the default /bin/sh. | None |
| stdin | The executed program's standard input file handles | None |
| stdout | The executed program's standard output file handles | None |
| stderr | The executed program's standard error file handles | None |
| preexec_fn | A callable object that is called just before the child process is executed. | None |
| close_fds | If close_fds is true, all file descriptors except 0, 1 and 2 will be closed before the child process is executed. (POSIX only). The default varies by platform: Always true on POSIX. On Windows it is true when stdin/stdout/stderr are None, false otherwise. On Windows, if close_fds is true then no handles will be inherited by the child process. Note that on Windows, you cannot set close_fds to true and also redirect the standard handles by setting stdin, stdout or stderr. | None |
| shell | The shell argument specifies whether to use the shell as the program to execute. If shell is True, it is recommended to pass args as a string rather than as a sequence. | False |
| cwd | Sets the current directory before the child is executed. | None |
| env | Defines the environment variables for the new process. | None |
| universal_newlines | If true, use universal line endings for file objects stdin, stdout and stderr. | False |
| encoding | Text mode encoding for file objects stdin, stdout and stderr. By default the stdout, stdin, stderr use bytes. | False |