### javascript 闭包 | Closures

  > javascript闭包在代码中很常见，熟练的掌握和运用闭包，能够帮助我们更好的构建高级应用。因此，我们需要更深层次的去了解它。

#### 作用域

  在JS中，变量的作用域分为2类：全局变量 和 局部变量。举个栗子

  ```javaScript
  var global = 'Global';

  function func() {
    var local = 'Local';

    // 在函数中，可以直接获取 全局变量。
    console.info(global);
  }

  func(); //正常运行，打印出 Global。

  //error, 无法获取函数内的局部变量。
  console.info(local);

  ```

#### 函数外读取局部变量

正常情况下，函数外无法直接读取函数内的局部变量。
因此，需要用特别的方式来获取。举个栗子

```javaScript

//定义函数作用域
function func() {
  var local = 'Local';

  // 定义一个子函数
  function childFunc() {
    //函数内打印local
    console.info(local);
  }
  return childFunc;
}

//执行func函数，returnFunc变量接收到childFunc函数
var returnFunc = func();

// 执行函数
returnFunc();

//打印结果为：Local
```

通过栗子可以得知，子函数childFunc虽然被return了，但仍可以访问到父函数func的内部变量<code>local</code>。(在JS中，这是合理的，函数的作用域在编写代码期间，就已经确定了，而非像某些其他语言，是在编译期)

#### 闭包的概念

上面的栗子就是一个典型的闭包，闭包可以实现访问某个函数的内部环境。

闭包由2部分构成：函数，以及函数所依赖的外部环境（作用域）。


> 一般来讲，当函数执行完毕后，局部活动对象就会销毁，内存仅保存全局作用域。但是，闭包的情况不同，closure函数执行完毕后，其活动对象不会销毁，因为闭包函数的作用域链仍然引用这个活动对象。直到闭包函数被销毁后，closure函数的活动对象才会被销毁。

> 由于闭包会携带包含它的函数的作用域，因此会占用更多的内存。

#### 闭包的用途

主要有2点：
  * 可以访问函数内部的环境。
  * 只要闭包引用不消失，闭包和其依赖的外部环境(作用域)会常驻内存。(一般函数在执行完毕后，即销毁了)

##### 私有变量和函数

> 仅通过给定方法进行操作，不希望部分函数内环境暴露出去(数据隐藏)。

```javascript
function Money() {
  // 我们不希望外部能直接修改金额。
  var money = 0;

  // 内部函数，仅函数内使用，不希望外部访问。
  function info() {
    console.info('the current money is ' + money);
  }

  // 增加金额，并做记录操作
  function add(num) {
    console.info('the money add '+ num);
    money += num;
    info();
  }

  // 减去金额，并做记录操作
  function sub(num) {
    console.info('the money sub '+ num);
    money -= num;
    info();
  }

  return {
    add: add,
    sub: sub
  }
}

var money = Money();
// 仅通过add/sub操作金额，而无法直接修改。
money.add(10);
money.sub(5);

```


##### 模仿块级作用域

> JavaScript没有块级作用域，用匿名函数可以用来模仿块级作用域并避免出现命名参数冲突的问题。
```JavaScript
(function(){  
 //块级作用域  
})();  
```

> 定义了一个匿名自执行函数，Javascript将function关键字当作函数声明的开始，而函数声明后面不能跟圆括号。然而，函数表达式的后面可以跟圆括号。要将函数声明转换为函数表达式，只要加上一对圆括号即可。

> 这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数，这种做法可以减少闭包占用内存的问题。
