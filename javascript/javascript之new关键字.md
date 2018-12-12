首先了解new这个关键字他做了哪些事儿？

1. 创建了一个新对象Object。
2. 将构造函数的作用域赋值给新对象（即将this指向新对象）
3. 执行构造函数里面的方法。
4. 返回新对象

下面，我们将自己实现一个new的功能：

```
function ownNew() {
  var obj = new Object(); //Object.create(null); //创建新对象
  var _constructor = Array.prototype.shift.call(arguments);
  _constructor.apply(obj, arguments); //改变this指向，指向新对象
  obj.__pro__ = _constructor.prototype; //设置原型
  return obj; //返回新对象
}
```

