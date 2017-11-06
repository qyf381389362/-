## JS面向对象

最近在看JS面向对象这块的内容，刚开始看有点复杂和混乱，理解的比较粗糙，所以摘出一些要点方便理解和以后查看。

笔记中的内容主要是从《JavaScript高级程序设计（第三版）》以及《JavaScript语言精粹（修订版）》摘出的。这两本书写的是着实不错，值得反复阅读。

在写面向对象编程之前，先把一些和函数有关的内容列出来，方便理解和构造函数以及继承中使用到的函数属性和方法。

### 函数

1. **关于return**

   如果函数中有return语句，那么这个函数在执行完return语句之后停止并立即退出。因此，位于return语句之后的任何代码都永远不会执行。return 语句可以不带有任何返回值，在这种情况下，函数在停止执行后将返回undefined值。这种用法一般用在提前停止函数执行而又不需要返回值的情况下。

2. **参数**

   JS中函数不介意传递进来多少参数，也不在乎传进来的参数式什么数据类型。。之所以会这样，原因是ECMAScript 中的参数在内部是用一个数组来表示的。函数接收到的始终都是这个数组，而不关心数组中包含哪些参数（如果有参数的话）。如果这个数组中不包含任何元素，无所谓；如果包含多个元素，也没有问题。实际上，在**函数体内可以通过arguments 对象来访问这个参数数组，从而获取传递给函数的每一个参数**。

   其实，arguments 对象只是与数组类似（它并不是Array 的实例），因为可以使用方括号语法访
   问它的每一个元素（即第一个元素是arguments[0]，第二个元素是arguments[1]，以此类推），使
   用length 属性来确定传递进来多少个参数。这个事实说明了ECMAScript 函数的一个重要特点：命名的参数只提供便利，但不是必需的。

   **ECMAScript 中的所有参数传递的都是值，不可能通过引用传递参数。**

3. **没有重载**


   ECMAScript 函数不能像传统意义上那样实现重载。而在其他语言（如Java）中，可以为一个函数编写两个定义，只要这两个定义的签名（接受的参数的类型和数量）不同即可。如前所述，ECMAScirpt函数没有签名，因为其参数是由包含零或多个值的数组来表示的。**而没有函数签名，真正的重载是不可能做到的**。
   如果在ECMAScript 中定义了两个名字相同的函数，则该名字只属于后定义的函数。请看下面的例子：

```javascript
function addSomeNumber(num){
	return num + 100;
}
function addSomeNumber(num) {
	return num + 200;
}
var result = addSomeNumber(100); //300
```

​	在此，函数addSomeNumber()被定义了两次。第一个版本给参数加100，而第二个版本给参数加
200。由于后定义的函数覆盖了先定义的函数，因此当在最后一行代码中调用这个函数时，返回的结果
就是300

​	如前所述，通过检查传入函数中参数的类型和数量并作出不同的反应，可以模仿方法的重载。

4. **检测类型**

   typeof 操作符是确定一个变量是字符串、数值、布尔值，还是undefined 的最佳工具。如果变
   量的值是一个对象或null，则typeof 操作符会像下面例子中所示的那样返回"object"：

   ```javascript
   var s = "Nicholas";
   var b = true;
   var i = 22;
   var u;
   var n = null;
   var o = new Object();
   alert(typeof s); //string
   alert(typeof i); //number
   alert(typeof b); //boolean
   alert(typeof u); //undefined
   alert(typeof n); //object
   alert(typeof o); //object
   ```

   虽然在检测基本数据类型时typeof 是非常得力的助手，但在检测引用类型的值时，这个操作符的
   用处不大。通常，我们并不是想知道某个值是对象，而是想知道它是什么类型的对象。为此，ECMAScript
   提供了instanceof 操作符。

   如果变量是给定引用类型的实例，那么instanceof 操作符就会返回true。请看下面的例子：

   ```javascript
   alert(person instanceof Object); // 变量person 是Object 吗？

   alert(colors instanceof Array); // 变量colors 是Array 吗？

   alert(pattern instanceof RegExp); // 变量pattern 是RegExp 吗？
   ```

   根据规定，所有引用类型的值都是Object 的实例。因此，在检测一个引用类型值和Object 构造
   函数时，instanceof 操作符始终会返回true。当然，如果使用instanceof 操作符检测基本类型的
   值，则该操作符始终会返回false，因为基本类型不是对象。

5. **执行环境及作用域**

   执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。

6. ​

7. **立即执行函数**

8. ​






