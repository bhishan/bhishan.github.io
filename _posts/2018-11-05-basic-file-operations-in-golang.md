---
title: 'Basic File Operations - Golang'
date: 2018-12-05
permalink: /posts/2018/12/basic-file-operations-golang/
tags:
  - Golang
  - Tutorials
  - File-System
---

One of the most basic task when working on a server is the ability to effectively operate with the files and file system. Like many languages, Golang has convenient methods to work with files. The intentions of this post is to host a minimalist set of examples on working with files using Golang.

## Creating an empty file

```go
package main

import (
  "log"
  "os"
)

func createFile(){
  newFile, err := os.Create("testfile.txt")
  if err != nil{
     log.Fatal(err)
  }
  defer newFile.Close()

  log.Println(newFile)
  // operations on file follows
}


func main(){
  createFile()
}
```

## Reading a File

```go
func readFile(){
  fileReader, err := os.Open("test.txt")
  if err != nil{
     log.Fatal(err)
  }
   defer fileReader.Close()
  content, err := ioutil.ReadAll(fileReader)
  if err != nil{
     log.Fatal(err)
  }
  fmt.Println("Number of bytes read", len(content))
  fmt.Printf("Data as string : %s", content)


}
```

## Writing to a file

```go
func writeToFile(){
  err := ioutil.WriteFile("testwrite.txt", []byte("Dumping bytes to a file\n"), 0666)
  if err != nil{
     log.Fatal(err)
  }
}
```

## Truncate File

```go
func TruncateFile(){
  err := os.Truncate("test.txt", 100)
  if err != nil{
     log.Fatal(err)
  }else{
     fmt.Print("File Truncated")
  }

}
```

## File Information

```go
func FileInformation(){
  fileInfo, err := os.Stat("test.txt")
  if err != nil {
     log.Fatal(err)
  }
  fmt.Println("File name:", fileInfo.Name())
  fmt.Println("Size in bytes:", fileInfo.Size())
  fmt.Println("Permissions:", fileInfo.Mode())
}
```

## Check if file exists or not

```go
package main

import (
    "os"
    "fmt"
)

func main(){
    if _, err := os.Stat("test.txt"); os.IsNotExist(err){
        fmt.Println("The file does not exist")
    return
    }
    fmt.Println("The file exists!")
    //perform operations on the file/file contents
}
```

## Move/Rename File

```go
package main
 
import (
    "log"
    "os"
)
 
func main() {
    currentFileLocation := "/home/bhishan-1504/golangfilehandling/test.txt"
    toMoveLocation := "/home/bhishan-1504/golangrestapi/test.txt"
    err := os.Rename(currentFileLocation, toMoveLocation)
    if err != nil {
        log.Fatal(err)
    }
}
```

## Copy File

```go
package main

import (
  "io"
  "log"
  "os"
  "fmt"
)

func main(){
  source, err := os.Open("test.txt")
  if err != nil{
    log.Fatal(err)
  }
  defer source.Close()

  sourceCopied, err := os.OpenFile("copiedtest.txt", os.O_RDWR|os.O_CREATE, 0666)
  //sourceCopied, err := os.Create("copiedtest.txt")
  if err != nil{
    log.Fatal(err)
  }
  defer sourceCopied.Close()

  _, err = io.Copy(sourceCopied, source)
  if err != nil{
    log.Fatal(err)
  }

 
  fmt.Println("Copied!")
}
```