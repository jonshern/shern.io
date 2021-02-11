+++
title = "Go Lang HTTP Testing and Mocking"
description = "Go Lang Testing and Mocking Techniques for HTTP."
author = "Jon Shern"
date = "2021-02-04"
tags = ["golang", "testing"]
categories = ["golang"]
[[images]]
  src = "img/2021/gotesting-red-green-refactor.jpg"
  alt = "Go Testing"
+++

Talks through some mocking strategies for http requests in Go Lang

<!--more-->

# H1
## HTTPTest

There are two main ways to use httptest to mock out a golang request

ResponseRecorder
and 
HTTP Server

### httptest / HTTPServer

This code is creating an http server that is running in memory.  It is possible to navigate to this site and look at the page.
You can go as far as you need to in creating a full page.
It is very flexible way to test http.


```
func TestHTTPSimple(t *testing.T) {

    server := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.WriteHeader(200)  //write an http status
        w.Write([]byte("Hello There"))
    }))


    //call the code that you want to test
    //inject the url
    
}
```


![Unit Testing](https://www.wisefamousquotes.com/images/unit-test-quotes-by-roy-osherove-1227390.jpg)
