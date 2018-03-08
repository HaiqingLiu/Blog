# jQuery.extend()和jQuery.fn.extend()区别和详解

### 1. 认识jQuery.extend()和jQuery.fn.extend()

jQuery的API手册中，extend()方法挂在jQuery和jQuery.fn两个不同对象上，但在jQuery内部代码实现是相同的，只是功能不太一样。

且看官方给出的解释：

jQuery.extend(): Merge the content of two or more objects together into the first object.（把两个或者更多的对象合并到第一个当中）；

jQuery.fn.extend():  Merge the content of an object onto the jQuery prototype to provide new jQuery instance methods.（把对象挂载到jQuery的prototype属性，来扩展一个新的jQuery实例方法）


### 2. 理解jQuery.extend()

我们先把jQuery看成一个类，这样好理解一些。jQuery.extend()是扩展这个类。

假如我们把jQuery这个类看成是人类，能吃饭能喝水能跑能跳，现在我们用jQuery.extend这个方法给这个类拓展一个能说话speak()的技能。这样的话，不论是男人、女人、xx人等能继承这个技能（方法）了。

可以如下这样写着：
```js
$(function(){
    $.speak();//函数调用
});

//函数拓展
(function($){
    $.extend({
        speak:function(){
            alert("how are you");
        }
    });
})(jQuery);
```

**这说明$.speak()变成了jQuery这个类本身的方法(object)，他现在能说话了。**

但是吧，这个能力啊，只有代表全人类这个类本身，才能用啊。你个人想用，你张三李四王五麻六，你个小草民能代表全人类吗？

所以啊，这个扩展也就是**所谓的静态方法，只跟这个类本身有关**。跟你具体的实例化对象是没关系的。

### 3. 理解jQuery.fn.extend()

从字面理解嘛，这个拓展的是jQuery.fn的方法。jQuery.fn是啥玩意儿呢？

```js
jQuery.fn = jQuery.prototype = {
    init:function(selector, context){
        //......
        
    }
}
```

所以jQuery.fn.extend()拓展的是jQuery对象（原型的）方法啊！

对象是啥？就是类的实例化嘛，例如$("#abc")、$(div)

那就是说，jQuery.fn.extend()方法你得用在jQuery对象上面才行啊！他得是张三李四王五麻六这些实例化的对象才能用啊。

说白了就是得这么用（假设xyz()是拓展的方法）：

```js
$('selector').xyz();
```

调用方法如下：
```js
$(function(){
    $("div").speak();//函数调用
});

//函数拓展
(function($){
    $.fn.extend({
        speak:function(){
            alert("how are you!");
        }
    });
})(jQuery);
```

### 4. 两者区别总结

#### 4.1 两者调用方式不同

**jQuery.extend():**一般由传入的全局函数来调用，主要用来拓展个全局函数的，如$.init()、$.ajax()。

**jQuery.fn.extend():** 一般由具体的实例对象来调用，可以用来拓展个选择器，例如$.fn.each()。

#### 4.2 两者的主要功能作用不同

**jQuery.extend():** 为扩展jQuery类本身，为自身添加新的方法。

**jQuery.fn.extend():** 给jQuery对象添加方法

#### 4.3 大部分插件都是用jQuery.fn.extend()

### 5.jQuery的extend扩展方法

#### 5.1 jQuery的扩展方法原型是：
extend(dest, src1, src2, src3...)

它的含义是将src1, src2, src3...合并到dest中，返回值为合并后的dest，
由此可以看出该方法合并后，是修改了dest的结构的。

如果想要得到合并的结果却又不想修改dest的机构，可以如下使用：
```js
var newSrc = $.extend({}, src1, src2, src3...);//也就是将"{}"作为dest参数
```
这样就可以将src1，src2,src3...进行合并，然后将合并结果返回给newSrc了。如下例：
```js
var result = $.extend({}, {name:"Tom", age:21}, {name:"Jerry", sex："Boy"});
```
那么合并后的结果：result = {name:"Jerry", age:21, sex:"Boy"}

也就是说，后面的参数如果和前面的参数存在相同的名称，那么后面的会覆盖前面的参数值。

#### 5.2 省略dest参数

上述的extend方法原型中的dest参数是可以省略的，如果省略了，则该方法就只能有一个src参数，而且是将改src合并到调用extend方法的对象中去，如：

##### 5.2.1 $.extend(src) 

该方法就是将src合并到jQuery的全局对象中去。

```js
$.extend({
    hello:function(){
        alert("hello");
    }
});
```

就是将hello方法合并到jQuery的全局对象中。

##### 5.2.2 $.fn.extend(src) 

该方法将src合并到jQuery的实例对象中去，如：

```js
$.fn.extend({
    hello:function(){
        alert("hello");
    }
});
```

就是将hello方法合并到jQuery的实例对象中。

下面列举几个常用的扩展实例：
```js
$.extend({net:{}}); //这是在jQuery全局对象中扩展一个net命名空间。

//这是将hello方法扩展到之前扩展的net命名空间中去。
$.extend($.net, {  
    hello:function(){
        alert("hello");
    }
}); 
```

##### 5.2.3 jQuery的extend方法还有一个重载原型：

extend(boolean, src1, src2, src3...)

第一个参数boolean代表是否进行深度拷贝，其余参数和前面介绍的一致，什么叫深层拷贝，我们看一个例子：

```js
var result=$.extend( true, {}, 
    { name: "John", location: {city: "Boston",county:"USA"} }, 
    { last: "Resig", location: {state: "MA",county:"China"} } 
); 
```

我们可以看出src1中嵌套子对象location:{city:"Boston"},src2中也嵌套子对象location:{state:"MA"},第一个深度拷贝参数为true，那么合并后的结果就是： 

```js
var result={
       name:"John",last:"Resig",
       location:{city:"Boston",state:"MA",county:"China"}
}
```

也就是说它会将src中的嵌套子对象也进行合并，而如果第一个参数boolean为false，我们看看合并的结果是什么，如下:

```js
 var result=$.extend( false, {}, 
       { name: "John", location:{city: "Boston",county:"USA"} },  
       { last: "Resig", location: {state: "MA",county:"China"} 
 }); 
```

那么合并后的结果就是:
 
```js
 var result={
      name:"John",last:"Resig",location:{state:"MA",county:"China"}
}
```

以上就是$.extend()在项目中经常会使用到的一些细节。

[参考]

https://www.cnblogs.com/xuxiuyu/p/5989743.html


