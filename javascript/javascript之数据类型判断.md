首先js的数据类型有：

1. 基本类型：string、number、boolean、null、undefined
2. 引用类型/复杂类型：Object、Array、Error、Date、RegExp、Function

目前判断数据类型有以下几种方法：

1. typeof
2. instanceof
3. constructor
4. Object.prototype.toString.call()

下面来详细说一下这几种方法：

1. ### typeof

   ```
   function printIn(val) {
     console.log(typeof val);
   }
   printIn(''); // string
   printIn(1); // number
   printIn(true); // boolean
   printIn(null); // object
   printIn(undefined); // undefined
   printIn({}); // object
   printIn([]); // object
   printIn(function(){}); // function
   printIn(new RegExp()); // object
   printIn(new Date()); // object
   printIn(new Error()); // object
   ```

   总结一下：

   - 基本类型：除了null，全都能返回数据的准确类型
   - 引用类型：除了function，全都返回object，都不能返回数据的准确类型
   - null 返回object
   - function 返回 function

   显而易见这不是我们想要的。

2. ### instanceof

   >  instanceof运算符用于测试构造函数的prototype属性是否出现在对象的原型链中的任何位置

   用法：A instanceof B.

   原理：判断B的prototype是否出现在对象A的原型链中的任何地方，简单的理解为：判断A是否是B的实例。



   ```
   var _date = new Date();
   var _err = new Error();
   function Person() {}
   var _p = new Person();
   
   console.log({} instanceof Object); // true;
   console.log([] instanceof Array); // true;
   console.log(_date instanceof Date); // true;
   console.log(_err instanceof Error); // true;
   console.log(_p instanceof Person); // true;
   ```

   现在我们稍微修改一下代码



   ```
   var _date = new Date();
   var _err = new Error();
   function Person() {}
   var _p = new Person();
   
   console.log({} instanceof Object); // true;
   console.log([] instanceof Object); // true;
   console.log(_date instanceof Object); // true;
   console.log(_err instanceof Object); // true;
   console.log(_p instanceof Object); // true;
   ```

   上面几种类型instanceof Object 都返回true，根据instanceof的原理所述，出现这样的情况并不难理解。有关原型和原型链方面的知识，大家可以阅读一下：  [javascript 原型和原型链](https://github.com/liyanging/articles/blob/master/javascript/javascript之原型.md)

   所以我们不难得出instanceof的不足之处：

   1. **instanceof 不适用于基本类型，只能用于引用类型。**
   2. **instanceof能判断出两者之间是否属于实例关系，但是不能判断出到底属于是那种类型**

3. ### constructor

   ```
   function Person() {};
   var _p = new Person();
   var _obj = {};
   var _num = new Number(1);
   console.log(''.constructor === String); //true
   console.log(true.constructor === Boolean); //true
   console.log(_num.constructor === Number); //true
   console.log([].constructor === Array); //true
   console.log(_obj.constructor === Object); //true
   console.log(_p.constructor === Person); //true
   
   ```

   了解过原型的同学都知道 constructor 值指向构造函数本身的，所以可以根据constructor来判断实例具体是属于哪个数据类型的；

   现在来修改一下代码

   ```
   function Person() {};
   Person.prototype = {}
   var _p = new Person();
   console.log(_p.constructor === Person)
   ```

   上面的代码可知，但用户手动的修改构造函数的prototype对象之后，实例的constructor变不指向该构造函数了;

   所以constructor存在的问题：

   1. **由于null 和 undefined是无效的对象，所以是不会有constructor的。**
   2. **当重写构造函数的原型对象后，原有的constructor指向便不存在了，都默认指向Object**

   所以在实际开发中，当重写了prototype对象之后，都需要重新指定constructor；

4. ### Object.prototype.toString.call()

   返回值："[object XXXX]"

   ```
   function printIn(val){
     console.log(Object.prototype.toString.call(val));
   }
   printIn(''); //[object String]
   printIn(1); //[object Number]
   printIn(true); //[object Boolean]
   printIn(null); //[object Null]
   printIn(undefined); //[object Undefined]
   printIn([]); //[object Array]
   printIn({}); //[object Object]
   printIn(new Date()); //[object Date]
   printIn(new Error()); //[object Error]
   ```

   个人喜欢用该方法判断对象类型；

