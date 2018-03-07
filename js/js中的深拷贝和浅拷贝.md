# js中的深拷贝和浅拷贝

### 1.浅拷贝

其实这段代码就是浅拷贝，有时候我们只是想备份数组，但是只是简单让它赋给一个变量，改变其中一个，另外一个就紧跟着改变，但很多时候这不是我们想要的。

```js
    var obj = {
        name:'wsscat',
        age:0
    };
    var obj2 = obj;
    obj2['c'] = 5;
    console.log(obj); //Object{name:'wsscat', age:0, c:5}
    console.log(obj2); //Object{name:'wsscat', age:0, c:5}
```

### 2.深拷贝

所谓深拷贝就是能够实现真正意义上的数组和对象的拷贝。

浅拷贝解决（深拷贝实现）：就是先设置一个新的对象obj2，通过遍历的方式将对象的值赋给obj2对象。

以最常见的数组和对象的深浅拷贝为例。

#### 2.1 数组

对于数组我们可以使用slice()和concat()来解决上面的问题。

###### 2.1.1 slice
```js
    var arr = ['spring', 'summer', 'autumn'];
    var arrCopy = arr.slice(0);
    arrCopy[0] = 'winter';
    console.log(arr); //['spring', 'summer', 'autumn'];
    console.log(arrCopy); //['winter', 'summer', 'autumn'];
```
###### 2.1.2 concat
```js
    var arr = ['spring', 'summer', 'autumn'];
    var arrCopy = arr.concat();
    arrCopy[0] = 'winter';
    console.log(arr); //['spring', 'summer', 'autumn'];
    console.log(arrCopy); //['winter', 'summer', 'autumn'];
```
###### 2.1.3 遍历
```js
    //数组的深拷贝实现
    var a = [1, 2, 3];
    var b = [];
    for(var i in a){
        b.push(a[i]);//或者b[i] = a[i];
    }
    b.push(4);
    console.log(a); //[1, 2, 3]
    console.log(b); //[1, 2, 3, 4]
```

#### 2.2对象

###### 2.2.1 对象深拷贝

对于对象，我们可以定义一个新的对象并遍历新的属性上去实现深拷贝。
```js
    var obj = {
        name:'wsscat',
        age:0
    }
    var obj2 = {};
    obj2.name = obj.name;
    obj2.age = obj.age;

    obj.name = 'spring';
    console.log(obj); //Object {name:'spring', age:0}
    console.log(obj2); //Object {name:'wsscat', age:0}
```

```js
    var obj = {
        'a':1,
        'b':2,
        'c':3
    };
    var obj2 = {};
    for(var i in obj){
        obj2[i] = obj[i];
    }
    obj2['d'] = 4;
    console.log(obj); //Object {'a':1, 'b':2, 'c':3}
    console.log(obj2); //Object {'a':1, 'b':2, 'c':3, 'd':4}
```

###### 2.2.2 对象更深层次拷贝实现

上面代码只能实现一层的拷贝，无法进行深层次的拷贝，封装函数再次通过对象数组嵌套测试如下：

```js
    //浅拷贝函数封装
    function shallowCopy(obj1, obj2) {
        for(var key in obj1){
            if(obj1.hasOwnProperty(key)){
                obj2[key] = obj1[key];
            }
        }
    }

    var obj1 = {
        fruits:['apple', 'banana'],
        number:100
    };
    var obj2 = {};
    shallowCopy(obj1, obj2);
    obj2.fruits[0] = 'orange';
    console.log(obj1.fruits[0]); //orange
    console.log(obj2.fruits[0]); //orange

```

结果证明，无法进行深层次的拷贝，这个时候我们可以使用深拷贝来完成。所谓**深拷贝**，就是能够实现真正意义上的数组和对象的拷贝，我们通过递归调用浅拷贝的方式实现。

代码封装实现如下：
```js
    function deepCopy(obj){
        //定义一个对象，用来判断当前的参数是数组还是对象
        var objArray = obj.isArray ? [] : {};
        //如果obj存在且类型为对象
        if(obj && typeof obj === 'object') {
            for (var key in obj){
                if(obj.hasOwnProperty(key)){
                    //如果obj的子元素是对象，递归操作
                    if(obj[key] && typeof obj[key] === 'object'){
                        objArray[key] = deepCopy(obj[key]);
                    }else{
                        //如果不是，直接赋值
                        objArray[key] = obj[key];
                    }
                }
            }
        }
        return objArray;//返回新的对象
    }

        var obj1 = {
            fruits:['apple', 'banana'],
            number:100
        };
        var obj2 = deepCopy(obj1);
        obj2.fruits[0] = 'orange';
        console.log(obj1.fruits[0]); //apple
        console.log(obj2.fruits[0]); //orange
        
        
        var obj11 =[{
            fruits:['apple', 'banana'],
            number:100
        }, 1];
        var obj22 = deepCopy(obj11);
        obj22[0].fruits[0] = 'orange';
        console.log(obj11[0].fruits[0]); //apple
        console.log(obj22[0].fruits[0]); //orange
        console.log(obj22);//对象数组
```
结果证明上面的代码可以实现深层次的拷贝。

###### 2.2.3 也可用JQuery下面的extend工具方法实现深层次拷贝

JQuery.extend([deep], target, object1, objecr[N]);
第一个参数设置为true,则JQuery返回一个深层次的副本，递归地复制找到的任何对象。

代码实现如下：

```js
        var obj1 ={
            fruits:['apple', 'banana'],
            number:100
        };
        var obj2 = $.extend(true, {}, obj1);
        obj2.fruits[0] = 'orange';
        console.log(obj1.fruits[0]); //apple
        console.log(obj2.fruits[0]); //orange
```


>其实对于javaScript的深拷贝和浅拷贝方法还有很多，这里只是介绍了常见的几种方式。

###### [参考] 

https://www.zhihu.com/question/23031215

https://github.com/Wscats/Good-text-Share/issues/57
