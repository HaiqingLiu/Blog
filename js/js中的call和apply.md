js中的call和apply
==


标签（空格分隔）： js

---

### 1.call和apply的用法

#### 1.1立即执行函数

```js
function hello(){
    return function(){
        console.log("hello");
    }
}

hello(); //hello
hello.call(); //hello
hello.apply(); //hello
```
#### 1.2改变this的指向

```js
var obj = {
    aaa:"this is obj"
};

function hello(){
    return function(){
        console.log(this.aaa);
    }
}

hello(); //undefined
hello.call(obj); //this is obj
hello.apply(obj); //this is obj
```

### 2.call vs apply

#### 2.1传递参数方式不同

```js
var obj = {
    word:"word"
};

function hello(a,b){
    console.log(a + " " + this.word + " " + b);
}

hello.call(obj, "first", "second"); //first word second
hell0.apply(obj, ["first", "second"]); //first word second
```
> call传递的是多个参数，apply传递的是参数数组。






