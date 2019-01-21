

## html input 上传文件夹 原始 

在线演示：https://jsfiddle.net/HaiqingLiu/jxz6w4po/


## html input 上传文件夹 自定义样式

- 上传同一文件change不触发解决方法（input[type=file]使用的是onchange去做，onchange监听的为input的value值，只有再内容发生改变的时候去触发，而value在上传文件的时候保存的是文件的内容，
你只需要在上传成功的回调里面，将当前input的value值置空即可。event.target.value=”）;

在线演示：https://jsfiddle.net/HaiqingLiu/tax0ferk/

【参考】

https://www.cnblogs.com/kongxianghai/p/5624785.html

https://blog.csdn.net/yang450712123/article/details/79266677
