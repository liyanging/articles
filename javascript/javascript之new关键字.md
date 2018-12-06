首先了解new这个关键字他做了哪些事儿？

1. 创建了一个新对象Object。
2. 设置原型
3. 改变this的指向，指向这个新对象。
4. 返回新对象

下面，我们将自己实现一个new的功能：

```
function ownNew() {
  var obj = Object.create(null); //创建新对象
  var _constructor = Array.prototype.shift.call(arguments);
  obj.__pro__ = _constructor.prototype; //设置原型
  _constructor.apply(obj, arguments); //改变this指向，指向新对象
  return obj; //返回新对象
}
```

