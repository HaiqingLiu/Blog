# js中Undefined和Null的区别


Undefined类型只有一个值就是undefined，Null类型也只有一个值就是null；
Undefined其实就是已声明未赋值的变量输出的结果；
Null其实就是一个不存在的对象的结果。
```js
var c;
console.log(c); //undefined
console.log(document.getElementById("wscat")); //没有id为wscat的节点，输出null
```




