## Using RxJava in many usage scenarios of Android

This sample demonstrates how to use  `rxjava in real business logic of android `  and  `common rxjava operators`.
I use local server with tomcat  to simulate real development [retrofit uses it].  you can download server code from github.
[server code](https://github.com/chiclaim/android_mvvm_server)


##Example Details

### 1. Basic Rxjava

Introducing rxjava operators like `create` 、`just` 、 `from`  、`map`、`flatMap`、`concatMap vs flatMap`



### 2. Every Http Reuest With Token [ if token was expire refresh it]  

Taking a token string to server When we started a http request in general. if the token is expire, server will return response code 401 , we must refresh the token from our server. Then resume our http request. Under the circumstances we implemented the logic in `callback hell`. 
we can use Rxjava to implement the logic with a elegant way.  
Using the operator `onErrorResumeNext` to do it.  Look the code for detail.





### 3. Search debounce

Almost all of the app has a search function.  We used to do this with addTextChangeListner of EditText Widget. We started a search http request when text changed. It will cause two problems:

* Comsuming user mobile traffic with many insignificant http request.

*  Outcome of the search is a trick. For example, user search key is 'AB' , it will cause two http request .  One http reqeust with search key 'A' , one with search key 'AB' . if request 'AB' is faster than reqeust 'A', the outcome of request 'A' will be overrided by the outcome of 'AB'.  It's a trick.  



Unfortunately, RxJava canot fix this problem comletely. We can use  `debounce` operator to reduce the occurrence of this kind of problem. For example, if user enter 'AB' to the edittext, it will product a http request and the server is processing.  At this very moment, user enter 'C' character in the input. It will product another http request with key word 'ABC'.  if 'ABC' request is faster than 'AB' request. The outcome of 'ABC' http request will be overrided by 'AB' http request.



We can use `debounce` operator to fix it. 



###  4. Observable is dependent on another Observable's result

For example , we want to upload a image to server.  first of all , we must retrieve the upload path(url).

Another example , we need to get IPs from hosts. we must retrieve hosts from server.



### 5. Check Data Cache



Retrieve data from Multiple Observables.  For example, getting data from memory,disk,network. which layer data not null,it will stop check the other layer. We can use  `concat` operator to handle this circumstance.



### 6. Retry http reqeust when occur error

We start a http request may get a error when server is starting or device wifi is enabling. we should retry http request.

We can set retry parameter like  `times`  and  `delay`. 





## Reference documents

1. [danlew posts](http://blog.danlew.net/page/6/)
2. [jianshu posts](http://www.jianshu.com/p/33c548bce571)
3. [myexception posts](http://www.myexception.cn/android/1949467.html)
4. [stackoverflow posts](http://stackoverflow.com/questions/26201420/retrofit-with-rxjava-handling-network-exceptions-globally)
