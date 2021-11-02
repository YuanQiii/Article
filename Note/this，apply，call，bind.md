# this，apply，call，bind

## 涵义

- `this`就是属性或方法“当前”所在的对象
- `this` 永远指向最后调用它的那个对象

## 实质

- 由于函数可以在不同的运行环境执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境（context）。
- 所以，`this`就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境

## 使用场合

- 全局环境使用`this`，它指的就是顶层对象`window`

  ```js
  this === window // true
  
  function f() {
    console.log(this === window);
  }
  f() // true
  ```

- 构造函数中的`this`，指的是实例对象

  ```js
  var Obj = function (p) {
    this.p = p;
  };
  
  var o = new Obj('Hello World!');
  o.p // "Hello World!"
  ```

- 如果对象的方法里面包含`this`，`this`的指向就是方法运行时所在的对象。该方法赋值给另一个对象，就会改变`this`的指向

  ```js
  var obj ={
    foo: function () {
      console.log(this);
    }
  };
  
  obj.foo() // obj
  
  var f = obj.foo
  f() // window
  ```

- 如果`this`所在的方法不在对象的第一层，这时`this`只是指向当前一层的对象，而不会继承更上面的层

  ```js
  var a = {
    p: 'Hello',
    b: {
      m: function() {
        console.log(this.p);
      }
    }
  };
  
  a.b.m() // undefined
  ```

## 使用注意点

- 避免多层 this

  - 由于`this`的指向是不确定的，所以切勿在函数中包含多层的`this`

    ```js
    var o = {
      f1: function () {
        console.log(this);
        var f2 = function () {
          console.log(this);
        }();
      }
    }
    
    o.f1()
    // Object
    // Window
    ```

    

  - 严格模式下，如果函数内部的`this`指向顶层对象，就会报错

    ```js
    var counter = {
      count: 0
    };
    counter.inc = function () {
      'use strict';
      this.count++
    };
    var f = counter.inc;
    f()
    // TypeError: Cannot read property 'count' of undefined
    ```

- 避免数组处理方法中的 this

  - 数组的`map`和`foreach`方法，允许提供一个函数作为参数。这个函数内部不应该使用`this`

    ```js
    var o = {
      v: 'hello',
      p: [ 'a1', 'a2' ],
      f: function f() {
        this.p.forEach(function (item) {
          console.log(this.v + ' ' + item);
        });
      }
    }
    
    o.f()
    // undefined a1
    // undefined a2
    // 内层的this不指向外部，而指向顶层对象
    ```

  - 解决方法

    ```js
    // 使用中间变量固定this
    var o = {
      v: 'hello',
      p: [ 'a1', 'a2' ],
      f: function f() {
        var that = this;
        this.p.forEach(function (item) {
          console.log(that.v+' '+item);
        });
      }
    }
    
    o.f()
    // hello a1
    // hello a2
    
    // 将this当作foreach方法的第二个参数，固定它的运行环境
    var o = {
      v: 'hello',
      p: [ 'a1', 'a2' ],
      f: function f() {
        this.p.forEach(function (item) {
          console.log(this.v + ' ' + item);
        }, this);
      }
    }
    
    o.f()
    // hello a1
    // hello a2
    ```

- 避免回调函数中的 this

  - 回调函数中的`this`往往会改变指向

    ```js
    var o = new Object();
    o.f = function () {
      console.log(this === o);
    }
    
    // jQuery 的写法
    $('#button').on('click', o.f);
    
    //f方法是在按钮对象的环境中被调用的
    ```

- this被作为函数调用的，而不是通过点符号被作为方法调用

  ```js
  function makeUser() {
    return {
      name: "John",
      ref: this
    };
  }
  
  let user = makeUser();
  
  alert( user.ref.name ); // Error: Cannot read property 'name' of undefined
  ```

- 箭头函数没有自己的 “this”

  - `this` 值取决于外部“正常的”函数

  - 箭头函数的`this` 始终指向函数定义时的 `this`，而非执行时
  
    ```js
    let group = {
      title: "Our Group",
      students: ["John", "Pete", "Alice"],
    
      showList() {
        this.students.forEach(
          student => alert(this.title + ': ' + student)
        );
      }
    };
    
    group.showList();
    // John
    // Pete
    // Alice
    ```
  

## 怎么改变 this 的指向

- 使用 ES6 的箭头函数
- 在函数内部使用 `_this = this`
- 使用 `apply`、`call`、`bind`
- new 实例化一个对象

## 绑定 this 的方法

- Function.prototype.call()

  - `call`方法格式

    ```js
    func.call(thisValue, arg1, arg2, ...)
    ```

  - 指定函数内部`this`的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数

    ```js
    var obj = {};
    
    var f = function () {
      return this;
    };
    
    f() === window // true
    f.call(obj) === obj // true
    ```

  - `call`方法的第一个参数，应该是一个对象。如果参数为空、`null`和`undefined`，则默认传入全局对象

    ```js
    var n = 123;
    var obj = { n: 456 };
    
    function a() {
      console.log(this.n);
    }
    
    a.call() // 123
    a.call(null) // 123
    a.call(undefined) // 123
    a.call(window) // 123
    a.call(obj) // 456
    ```

  - 如果`call`方法的第一个参数是一个原始值，那么这个原始值会自动转成对应的包装对象，然后传入`call`方法

    ```js
    var f = function () {
      return this;
    };
    
    f.call(5)
    // Number {[[PrimitiveValue]]: 5}
    ```

  - `call`方法的一个应用是调用对象的原生方法，它将`hasOwnProperty`方法的原始定义放到`obj`对象上执行，这样无论`obj`上有没有同名方法，都不会影响结果

    ```js
    var obj = {};
    obj.hasOwnProperty('toString') // false
    
    // 覆盖掉继承的 hasOwnProperty 方法
    obj.hasOwnProperty = function () {
      return true;
    };
    obj.hasOwnProperty('toString') // true
    
    Object.prototype.hasOwnProperty.call(obj, 'toString') // false
    ```

- Function.prototype.apply()

  - `apply`方法格式

    ```js
    func.apply(thisValue, [arg1, arg2, ...])
    ```

  - `apply`方法的作用与`call`方法类似，也是改变`this`指向，然后再调用该函数。唯一的区别就是，它接收一个数组作为函数执行时的参数

  - 应用

    - 找出数组最大元素

      ```js
      var a = [10, 2, 4, 15, 9];
      Math.max.apply(null, a) // 15
      ```

      

    - 将数组的空元素变为`undefined`，空元素与`undefined`的差别在于，数组的`forEach`方法会跳过空元素，但是不会跳过`undefined`

      ```js
      Array.apply(null, ['a', ,'b'])
      // [ 'a', undefined, 'b' ]
      ```

    - 转换类似数组的对象

      ```js
      Array.prototype.slice.apply({0: 1, length: 1}) // [1]
      Array.prototype.slice.apply({0: 1}) // []
      Array.prototype.slice.apply({0: 1, length: 2}) // [1, undefined]
      Array.prototype.slice.apply({length: 1}) // [undefined]
      ```

    - 绑定回调函数的对象

      ```js
      var o = new Object();
      
      o.f = function () {
        console.log(this === o);
      }
      
      var f = function (){
        o.f.apply(o);
        // 或者 o.f.call(o);
      };
      
      // jQuery 的写法
      $('#button').on('click', f);
      ```

- Function.prototype.bind()

  - `bind()`方法用于将函数体内的`this`绑定到某个对象，然后返回一个新函数

    ```js
    var counter = {
      count: 0,
      inc: function () {
        this.count++;
      }
    };
    
    var func = counter.inc.bind(counter);
    func();
    counter.count // 1
    ```

  - `bind()`还可以接受更多的参数，将这些参数绑定原函数的参数

  - `bind()`方法的第一个参数是`null`或`undefined`，等于将`this`绑定到全局对象

    ```js
    function add(x, y) {
      return x + y;
    }
    
    var plus5 = add.bind(null, 5);
    plus5(10) // 15
    ```

- 注意点

  - 每一次返回一个新函数

    ```js
    element.addEventListener('click', o.m.bind(o));
    element.removeEventListener('click', o.m.bind(o));// 无效
    
    // 正确的方法
    var listener = o.m.bind(o);
    element.addEventListener('click', listener);
    //  ...
    element.removeEventListener('click', listener);
    ```

  - 结合回调函数使用

    ```js
    var counter = {
      count: 0,
      inc: function () {
        'use strict';
        this.count++;
      }
    };
    
    function callIt(callback) {
      callback();
    }
    
    callIt(counter.inc.bind(counter));
    counter.count // 1
    ```

    ```js
    var obj = {
      name: '张三',
      times: [1, 2, 3],
      print: function () {
        this.times.forEach(function (n) {
          console.log(this.name);
        }.bind(this));
      }
    };
    
    obj.print()
    // 张三
    // 张三
    // 张三
    ```

  - 结合`call()`方法使用

    ```js
    [1, 2, 3].slice(0, 1) // [1]
    // 等同于
    Array.prototype.slice.call([1, 2, 3], 0, 1) // [1]
    
    // call()方法实质上是调用Function.prototype.call()方法
    var slice = Function.prototype.call.bind(Array.prototype.slice);
    slice([1, 2, 3], 0, 1) // [1]
    ```

    

## 参考

- [网道-this关键字](https://wangdoc.com/javascript/oop/this.html)
- [现代 JavaScript 教程-对象方法，“this”](https://zh.javascript.info/object-methods)
- [现代 JavaScript 教程-深入理解箭头函数](https://zh.javascript.info/arrow-functions)

