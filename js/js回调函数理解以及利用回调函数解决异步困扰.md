# js回调函数理解以及利用回调函数解决异步困扰



## 一、js回调函数理解

### 1.前奏

在谈回调函数前先看一下下面两段代码，并猜测下结果。
```js
function say(value){
    console.log(value);
}

console.log(say);//function say(value){ … }

var a = say('haha');//a为undefined，因为函数无返回值
console.log(a);//'haha' 'undefined'
```

```js
function say(value){
    console.log(value);
    return value;
}

console.log(say);//function say(value){ … }

var a = say('haha');//函数有返回值，a为'haha'
console.log(a);//'haha' 'haha'
```

**测试发现：**

 - 只写变量名say，返回的将会是say方法本身，以字符串的形式表现出来。
 - 而在变量名后加()如say()返回的就会是say方法调用后的结果，这里是打印value的值。

### 2.js中函数可以作为参数传递

再看下面两段代码：
```js
function say(value){
    console.log(value);
}

function execute(callBack, value){
    console.log('xixi');
    callBack(value);
}

execute(say, 'haha');//'xixi' 'haha'
```

```js
function execute(callBack, value){
    callBack(value);
}

execute(function(value){
    console.log(value);
},'haha');//'haha'
```
上面第一段代码是将say方法作为参数传递给execute方法，
第二段代码则是直接将匿名函数作为参数传递给execute方法。

实际上：
```js
function say(value){
    console.log(value);
}
//注意看下面，直接写say方法的方法名与下面的匿名函数可以认为是一个东西
//这样再看上面两段代码是不是对函数可以作为参数传递就更加清晰了。
say;

function (value){
    console.log(value);
}
```

  这里say或者匿名函数就被称为**回调函数**。
  
 
后记：
```js
//匿名函数（立即执行函数）一种正确形式如下
(function (value){
    console.log(value);
}('haha'));
```

### 3.回调函数易混淆点 ———— 传参
如果回调函数需要传参，下面介绍两种解决方案。

- 将回调函数的参数作为与回调函数同等级的参数进行传递
```js
function say(value){                <=====  //回调函数需要一个参数value
    console.log(value);
}

function execute(callBack, value){           //execute的第一个参数应该是一个函数，但确是函数本身       
    console.log('xixi');            <=====   //（可以认为在函数外加上双引号直接变为字符串),在函数本身
    callBack(value);                         //后面加上()就是运行这个函数，如果需要参数，就在 这个时候传入。
}                                                     
                                                                                                            
execute(say, 'haha');                       //这里的say方法就是回调函数,这里的execute方法接收了两个参数，第一个参数
                                    <=====  //是say,第二个参数是啊'haha'。这里的第二个参数其实就是say方法需要的参数。
                                            //这样写就叫做“将回调函数的参数作为与回调函数同等级的参数进行传递”。
                                                  
```

- 回调函数的参数在调用回调函数内部创建
```js
function say(value){                <=====  //回调函数需要一个参数value,这个value是一个对象，拥有属性name。
    console.log(value.name);        
}                                   

function execute(callBack){      
    var value = {//这里可以自定义一些方法       //这里execute函数只接收一个参数即say函数本身，
        name:'haha'                 <=====   //say函数的参数在execute函数内部已经定义过了。
    };                                       //这里直接传递也能达到回调函数传参的目的。
    callBack(value);                         
}                                   
                                    
                                    
execute(say);                       <=====   //excute()函数是主函数，参数为say函数本身,这儿的say函数也叫回调函数。

```                       
  
## 二、利用回调函数解决异步困扰

虽然已经存在promise等工具来解决回调地狱，但是，讲真，我觉得他们并没有让代码的可读性大大增强，而且在回调函数的嵌套次数有限的情况下也不至于成为一个
“地狱”，所以笔者还是老老实实的继续啃这块js里的板砖————**利用回调函数解决js异步困扰**。

例子如下：

```js
'use strict'
fun(function(data){//data:num1
    fun1(data, function(data){//data:num2
        fun2(data,function(data){//data:num3
            fun3(data);
        });
    });
});

function fun(callback){
    var num1 = 1;
    console.log("begins!");
    callback(num1);
}

function fun1(num1, callBack){
    var num2 = 2;
    setTimeout(function(){
        console.log(num1);
        callBack(num2);
    }, 3000);
}

function fun2(num2, callBack){
    var num3 = 3;
    setTimeout(function(){
        console.log(num2);
        callBack(num3);
    }, 3000);
} 

function fun3(num3){
    setTimeout(function(){
        console.log(num3);
    }, 3000);
}
```

输出结果如下： 
begins!
1
2
3

可以看出函数的执行顺序为fun1--->fun2--->fun3，是按照我们回调函数嵌套，由外层向内层执行，达到了某种意义上的顺序执行。

**注意：**这里用定时器是为了执行时体现出执行顺序，若不加定时器也是按此顺序执行，只是视觉上没那么直观。

【参考】
js 彻底理解回调函数
https://blog.csdn.net/baidu_32262373/article/details/54969696

javascript利用回调函数解决异步困扰
https://blog.csdn.net/memeda_bupt/article/details/52267294
