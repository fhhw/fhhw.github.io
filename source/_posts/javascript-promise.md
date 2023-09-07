---
title: JavaScript 异步编程 Promise
date: 2021-07-22 10:51:15
tags: JavaScript
categories: JavaScript
cover: /img/js/js_logo.png
description: 通常来说，程序都是顺序执行的，同一时刻只会发生一件事。如果一个函数依赖于另一个函数的结果，它只能等待那个函数结束才能继续执行，从用户的角度来说，整个程序才算运行完毕。 
---

## 异步编程
通常来说，程序都是顺序执行的，同一时刻只会发生一件事。如果一个函数依赖于另一个函数的结果，它只能等待那个函数结束才能继续执行，从用户的角度来说，整个程序才算运行完毕。  
### 线程
一个线程是一个基本的处理过程，程序用它来完成任务。每个线程一次只能执行一个任务:  
```
Task A --> Task B --> Task C  
```
每个任务顺序执行，只有前面的结束了，后面的才能开始。  
现在的计算机大都有多个内核（core），因此可以同时执行多个任务。支持多线程的编程语言可以使用计算机的多个内核，同时完成多个任务: 
```
Thread 1: Task A --> Task C  
Thread 2: Task B --> Task D  
```
一个程序，通常从Main方法开始，我们称此线程称为主线程（main thread）。
### 异步概念
如果程序调用某个方法，等待其执行全部处理后才能继续执行，我们称其为同步的。相反，在处理完成之前就返回调用方法则是异步的。  
在使用同步编程方式时，由于每个线程同时只能发起一个请求并同步等待返回，这种等待不仅使程序变慢，而且浪费了计算机资源，所以为了提高系统性能，此时我们就需要引入更多的线程来实现并行化处理，这就是异步编程的出发点。  
异步编程是可以让程序并行运行的一种手段，其可以让程序中的一个工作单元与主应用程序线程分开独立运行，并且在工作单元运行结束后，会通知主应用程序线程它的运行结果或者失败原因。使用异步编程可以提高应用程序的性能和响应能力等。  
### 异步编程的分类
JavaScript 传统上是单线程的，经过不断完善，JavaScript 获得了一些工具来帮助解决这种问题。解决异步问题方法大致包括：直接回调、pub/sub模式(事件模式)、异步库控制库(例如async、when)、promise、Generator等。  
## Promise 对象
Promise 是异步编程的一种解决方案，比传统的解决方案回调函数和事件更合理和更强大。  
Promise对象有以下两个特点：  
+ 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
+ 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### 基本用法
Promise使用实例：
```js
function asyncDo(){
    return new Promise(function(resolve,reject){
        // some codes
        if(/*异步操作成功*/){
            resolve(value); //异步操作成功，将value传递出去
        }else{
            reject(erroe);  //异步操作失败，报出错误，将error传递出去
        }
    });
}
```
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。  
resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。  
Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。  
```js
asyncDo().then(function(value){
    //value为异步结果成功时，resolve传递出的参数
    //对异步结果成功时的操作
}，function(error){
    //error为异步结果失败时，reject传递出的参数
    //对异步结果失败时的操作    
});
```
then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。这两个函数都是可选的，不一定要提供。它们都接受Promise对象传出的值作为参数。  
一般来说，调用resolve或reject以后，Promise 的使命就完成了，后继操作应该放到then方法里面，而不应该直接写在resolve或reject的后面。所以，最好在它们前面加上return语句，这样就不会有意外。  
```js
new Promise((resolve, reject) => {
  return resolve(1);
  // 后面的语句不会执行
  console.log(2);
})
```
### Promise.prototype.then()
Promise 实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为 Promise 实例添加状态改变时的回调函数。then方法返回的是一个新的Promise实例，因此可以采用链式写法，即then方法后面再调用另一个then方法。
```js
getJSON("./1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function (comments) {
  console.log("resolved: ", comments);
}, function (err){
  console.log("rejected: ", err);
});
```
上面代码中，第一个then方法指定的回调函数，返回的是另一个Promise对象。这时，第二个then方法指定的回调函数，就会等待这个新的Promise对象状态发生变化。如果变为resolved，就调用第一个回调函数，如果状态变为rejected，就调用第二个回调函数。  
### Promise.prototype.catch()
Promise.prototype.catch()方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。  
```js
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```
上面代码中，getJSON()方法返回一个 Promise 对象，如果该对象状态变为resolved，则会调用then()方法指定的回调函数；如果异步操作抛出错误，状态就会变为rejected，就会调用catch()方法指定的回调函数，处理这个错误。另外，then()方法指定的回调函数，如果运行中抛出错误，也会被catch()方法捕获。  
### Promise.prototype.finally() 
finally()方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。  
```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```
上面代码中，不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。
****
详细内容参见[Promise 对象](https://es6.ruanyifeng.com/#docs/promise)
