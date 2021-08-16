---
title: Promise-异步编程的一种解决方案
---

## 1.Promise的含义
Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件更合理且更强大。
简单来说，Promise是一个容器，里面包含着一个未来才会结束的事件。从语法上，promise是一个对象，从它可以获取异步操作的消息。
### 1.1 Promise 的特点
Promise有两个特点。
	1. promise对象的状态不受外界影响。promise有三种状态，Pending(进行中)，Fulfilled(已完成)，Rejected(已失败) 只有异步操作的结果才能决定当前是哪种状态，其他操作无法改变。
	2. 一旦状态改变就不会再变。任何时候都可以得到这个结果。
		一般状态改变有两种可能：Pending -> Fulfilled 或 Pending -> Rejected
		一旦改变，状态就定型了，也被称为Resolved，会一直保持这个状态。
	而这与事件完全不同，错过了事件触发的时机，就再也监听不到了，而Promise可以一直监听状态。
## 2. Promise的基本用法
```javascript
var promise = new Promise(function(resolve,reject) {
	// 一些代码...
	if(/* 异步操作成功*/){
	resolve(value)
} )else{
	reject(error)
}
```
Promise构造函数接受一个函数作为参数，这个函数有两个参数，分别为resolve，reject
这两个参数也为函数，由JavaScript引擎提供，不用自己部署。
	**resolve函数的作用是**，将Promise对象的状态由 Pending 变为 Fulfilled 在异步操作成功时调用，并将异步操作的结果作为参数返回出去（value）。
	**相对的，reject的作用是**，将Promise对象的状态由 Pending 变为 Rejected 在异步操作失败时调用，并抛出错误（error）。
###  2.1 then（）方法
Promise实例生成后，可以用then()方法为实例对象指定 resolve 和 reject 的具体操作。
如下：

```javascript
// promise 为上一段代码的Promise实例对象
promise.then(function(value){
	//fulfilled状态下执行的代码
},function(error){
	// rejected状态下执行的代码
	// 一般情况下这里会抛出错误
})
```
then()方法可以指定 resolve 和 reject ，第一个参数为 resolve，第二个参数为 reject，第二个参数**可选**。但是一般情况下只在then()中为 resolve 指定操作，不会为 reject 指定具体操作。
### 2.2 catch()方法
上面说到，一般不会在then()中指定 reject ，这里说明 reject 的定义时机。
catch()方法专门用来指定发生错误时的回调函数。

```javascript
Promise.prototype.catch 方法的别名是 .then(null,rejection)
```
调用方式
```javascript
promise.catch(function(error){
	console.log('发生错误！')
})
```
### 2.3 then() 和 catch（）的综合使用

```javascript
// 第一种使用方式
promise.then(function(value){
	//fulfilled状态下执行的代码
},function(error){
	// rejected状态下执行的代码
	// 一般情况下这里会抛出错误
})
// 第二种使用方式
promise.then(function(data){
	// success
}).catch(function(err){
	// error
})
```
因为promise对象的错误具有冒泡性质，错误会一直向后传递，直到捕获为止。后一个catch()总会捕捉到前面的错误，如果按照第一种使用方式，若是 resolve 或 reject 中同样出现了错误，这时由于后面再没有catch()来捕获异常，这个错误将成为无法捕获的错误。
所以说，第二种方法优于第一种，理由是第二种写法可以捕获前面then方法的异常，也更接近同步的写法（try/catch）

```javascript
promise..catch(function(err){
	// error
	}).then(function(data){
	// success
})
```

**then方法和 catch方法都返回的是promise对象，**因此后面还可以接着调用then()或catch()
如上代码，若是catch()方法在then()之前，上面代码因为没有报错而跳过 catch 方法，直接执行了后面的 then 方法。 此时要是 then 方法里面报错，就与前面的 catch 无关了。**因此catch方法应尽量放在then方法后。**
## 3. Promise的其他常用方法
待总结。。。
