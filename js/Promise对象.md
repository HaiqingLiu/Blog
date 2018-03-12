# Promise对象

标签（空格分隔）： js

---

在JavaScript的世界中，所有代码都是**单线程**执行的。

由于这个“缺陷”，导致JavaScript的所有网络操作、浏览器事件，都必须是**异步执行**。**异步执行**可以用**回调函数**实现：
```js
function callback(){
    console.log('Done');
}

console.log('before setTimeout()');
setTimeout(callback, 1000);//一秒钟后调用callback函数
console.log('after setTimeout()');
```
观察上述代码执行，在控制台输出可以看到：

>before setTimeout()
after setTimeout()
（等待1秒后）
Done

可见，异步操作会在将来的某个时间点触发一个函数调用。

AJAX就是典型的异步操作。以上一节的代码为例

```js
request.onreadystatechange  = function(){
    if(request.readyState === 4){
        if(request.status === 200){
            return success(request.responseText);
        }else{
            return fail(request.status);
        }
    }
}
```

把回调函数success(request.responseText)和fail(request.status)写到一个AJAX操作里很正常，但是不好看，而且不利于代码复用。

有没有更好的写法？比如写成这样？

```js
var ajax = ('http://...');
ajax.ifSuccess(success)
    .ifFail(fail);
```

这种链式写法的好处在于，先统一执行AJAX逻辑，不关心如何处理结果，然后根据结果是成功还是失败，在将来的某个时候调用success函数或者fail函数。

这种“承诺将来会执行”的对象在JavaScript中称为**Promise对象**。

Promise有各种开源实现，在ES6被统一规范，由浏览器直接支持。先测试一下你的浏览器是否支持Promise()：
```js
'use strict'
new Promise(function(){});
//直接运行测试
console.log('支持Promise');
```

我们先看一个最简单的Promise例子：生成一个0-2之间的随机数，如果小于1,则等待一段时间后返回成功，否则返回失败：
```js
function test(resove, reject){
    var timeOut = Math.random() * 2;
    console.log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function(){
        if(timeOut < 1){
            console.log('call resove()...');
            resove('200 ok');
        }else{
            console.log('call reject()...');
            reject('timeout in' + timeOut + 'seconds.');
        }
    }, timeOut*1000);
}
```

这个test函数有两个参数，这两个参数都是函数，如果执行成功，我们将调用resove('200 ok')，如果执行失败，我们将调用reject('timeout in' + timeOut + 'seconds.')。可以看出，test()函数只关心自身的逻辑，并不关心具体的resove和reject将如何处理结果。

有了执行函数我们就可以用一个Promise对象来执行它，并在将来某个时刻获得成功或失败的结果。
```js
var p1 = new Promise(test);
var p2 = p1.then(function(result){
    console.log('成功：' + result);
});
var p3 = p2.then(function(reason){
    console.log('失败：' + reason);
});
```

变量p1是一个Promise对象，它负责执行test函数。由于test函数在内部是异步执行的，当test函数执行成功时，我们告诉Promise对象：

```js
//如果成功，执行这个函数
p1.then(function(result){
    console.log('成功：' + result);
});
```
当test函数执行成功时，我们告诉Promise对象：

```js
p2.catch(function(reason){
    console.log('失败：' + reason);
});
```

Promise对象可以串联起来，所以上述代码可以简化为：

```js
new Promise(test).then(function(result){
    console.log('成功：' + result);
}).catch(function(reason){
    console.log('失败：' + reason);
});
```

可见Promise最大的好处是在异步执行的流程中，把执行代码和处理结果的代码清晰地分离了。

Promise还可以做更多的事情，比如，有若干个异步任务，需要先做任务1，如果成功后再做任务2，任何任务失败则不在继续并执行错误处理函数。

要串行执行这样的异步任务，不用Promise需要写一层一层的嵌套代码。有了Promise，我们只需要简单地写：
```js
job1.then(job2).then(job3).catch(handleError);
```

其中job1、job2和job3都是Promise对象。





【参考】

https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014345008539155e93fc16046d4bb7854943814c4f9dc2000

http://wiki.jikexueyuan.com/project/es6/promise.html


