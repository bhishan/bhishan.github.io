---
title: 'Zip Files using Python'
date: 2018-07-17
permalink: /posts/2018/07/zip-files-using-python/
tags:
  - Python
  - File Compression
  - ZIP
  - Shutil
  - ZipFile
---

Zipping files can be one part of a more complex operations that we perform using programming. This can usually happen when you are working on a data pipeline and/or products requiring data movement. Python has easy methods available for zipping files and directories. For the records, a ZIP is an archive file format that supports lossless data compression. A ZIP file may contain one or more files or directories that may have been compressed.

## How to archive files/directories using shutil?
The shutil module offers a number of high-level operations on files and collections of files. Following code block will zip the files and directories present in the the source directory provided as the third argument to the make_archive function from shutil.

```python
>>> from shutil import make_archive
>>> make_archive("July17-2018", "zip", "/home/bhishan-1504/shutil_test_archive")
```

## Details about the parameters of make_archive function:
**base_name** : It is the name of the file to create. This filename is expected to be without the format specific extension.

**format** : It is the archive format which could be one of "zip", "tar", "gztar", "bztar" or any other registered format.

**root_dir** : It is the directory that will be the root directory of the archive i.e we typically chdir into 'root_dir' before creating the archive.

**base_dir** : It is the directory where we start archiving from; ie. 'base_dir' will be the common prefix of all files and directories in the archive.

The make_archive function returns the filename of the archived file. Note that owner and group are used when creating a tar archive. By default, it uses the current owner and group.

## How to archive selective files/directories using zipfile?
We also have control over what files and directories should be archived rather than the entire directory tree. This can be achieved by the following code block:

```python
>>> from zipfile import ZipFile
>>> with ZipFile("testarchive.zip", "w") as zip_buff:
...     zip_buff.write("1.txt")
...     zip_buff.write("3.txt")
...
>>>
```

All we do is write to the ZipFile object the files to be archived.

## Details about the parameters of the ZipFile class:
**file**: Either the path to the file, or a file-like object. If it is a path, the file will be opened and closed by ZipFile.

**mode**: The mode can be either read "r", write "w" or append "a".

**compression**: ZIP_STORED (no compression) or ZIP_DEFLATED (requires zlib).

**allowZip64**: if True ZipFile will create files with ZIP64 extensions when needed, otherwise it will raise an exception when this would be necessary.