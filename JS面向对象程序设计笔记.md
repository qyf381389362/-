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

   执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。某个执行环境中的所有代码执行完
   毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）。

   **JS中没有块级作用域**

   使用var 声明的变量会自动被添加到最接近的环境中。在函数内部，最接近的环境就是函数的局部环境；在with 语句中，最接近的环境是函数环境。如果初始化变量时没有使用var 声明，该变量会自动被添加到全局环境。如下所示：


   ```javascript
   function add(num1, num2) {
     var sum = num1 + num2;
     return sum;
   }
   var result = add(10, 20); //30
   alert(sum); //由于sum 不是有效的变量，因此会导致错误
   ```

   如果省略这个例子中的var 关键字，那么当add()执行完毕后，sum 也将可以访问到：

   ```javascript
   function add(num1, num2) {
     sum = num1 + num2;
     return sum;
   }
   var result = add(10, 20); //30
   alert(sum); //30
   ```


4. **闭包**


   闭包是一个抽象概念，它不存在对应的语法。如下，die函数用到你外面的变量lives，就是闭包。

   ```javascript
   function xxx(){
      var lives = 30;
      function die(){
          lives -= 1;
          return lives;
      }
      return die;         
   }
   var dieFn = xxx();        
   var currentLives = dieFn();
   ```

   结论：**一个函数用到了它外面的变量就是闭包**，即一个函数return了它外面的变量。这种用法叫做闭包。

   **闭包的作用：**

   让别人可以**“间接访问”**。换句话说，**''隐藏一个变量"**。

   上面xxx这个函数是我们在做一个游戏，在写其中关于「还剩几条命」的代码。 如果不用闭包，你可以直接用一个全局变量：

   ```javascript
    windows.lives = 30  //还有30条命
   ```

   上面这行代码 lives 是一个全局变量谁都可以访问，万一不小心把windows.lives的值改成 -1 了怎么办。所以我们不能让别人「直接访问」这个变量。怎么办呢？**用xxx函数，造出一个局部变量。但又为了 [别人又能够访问] 到这个变量,并且实现 [游戏中的减命] 功能， 用die()函数return lives变量的值那么别人就可以间接访问到这个隐藏变量。**



​	**闭包造成内存泄漏：**

​	内存泄露是指你用不到（访问不到）的变量，依然占居着内存空间，不能被再次利用起来。

    	闭包里面的变量明明就是我们需要的变量（lives），所以不会出现内存泄漏。

    	有人说闭包会造成内存泄漏，什么情况？

    	因为 IE。IE 有 bug，IE 在我们使用完闭包之后，依然回收不了闭包里面引用的变量。这是 IE 的问题，不是闭包的问题。

5. **立即执行函数**

   1. 什么是立即执行函数

      声明一个匿名函数，立即执行它。

      ```javascript
      !function(){
         var lives =30
         console.log(lives)
       }.call()
      ```

      保留字function前面感叹号可以换成 + - ~ 等符号，也可以换成括号。

   2. 立即函数的作用

      作用只有一个：**创建一个独立的作用域。**这个作用域里面的变量，外面访问不到（**避免了变量污染**）。

      所以上面[还剩几条命的代码] 除了使用[闭包]还可以改写成立即执行函数：

      ```javascript
      ！function (){
         var lives = 30
         function die(){
             lives -= 1
             return lives
         }
         return die
      }.call()
      ```

      看一道经典的面试题：

      ```javascript
      var liList = ul.getElementsByTagName('li')
      for(var i=0; i<6; i++){
        liList[i].onclick = function(){
          alert(i); // 为什么 alert 出来的总是 6，而不是 0、1、2、3、4、5
        }
      }
      ```

      为什么每次alert结果都是6呢？

      因为i是贯穿整个作用域的（即window下的），而不是给每个div分配一个i。for循环执行完后i已经变成6了（注意不是5），然后用户一定是在for循环运行完之后才点击的，此时i的结果已经为6了。

      那我们怎么解决这个问题呢？明显我们要给每个li创建一个独立的作用域，一种方法就是用[立即执行函数]。

      ```javascript
      var liList = ul.getElementsByTagName('li')
      for(var i=0; i<6; i++){
        !function(ii){
          liList[ii].onclick = function(){
            alert(ii) // 0、1、2、3、4、5
          }
        }(i)
      }
      ```

      在立即执行函数执行的时候，i 的值被赋值给 ii，此后 ii 的值一直不变。

      i 的值从 0 变化到 5，对应 6 个立即执行函数，这 6 个立即执行函数里面的 ii 「分别」是 0、1、2、3、4、5。

---

### 对象

#### **创建对象的方式**

1. **通过object创建**

   ```javascript
   var person = new Object();
   person.name = "Nicholas";
   person.age = 29;
   person.job = "Software Engineer";
   person.sayName = function(){
   alert(this.name);
   };
   ```

2. **对象字面量**

   ```javascript
   var person = {
   	name: "Nicholas",
   	age: 29,
   	job: "Software Engineer",
   	sayName: function(){
   		alert(this.name);
   	}
   };	
   ```

3. **工厂模式**

   虽然Object 构造函数或对象字面量都可以用来创建单个对象，但这些方式有个明显的缺点：使用同一个接口创建很多对象，会产生大量的重复代码。为解决这个问题，人们开始使用工厂模式的一种变体。

   考虑到在ECMAScript 中无法创建类，开发人员就发明了一种函数，用函数来封装以特定接口创建对象的细节，如下面的例子所示。

   ```javascript
   function createPerson(name, age, job){
   	var o = new Object();
   	o.name = name;
   	o.age = age;
   	o.job = job;
   	o.sayName = function(){
   		alert(this.name);
   	};
   	return o;
   }
   var person1 = createPerson("Nicholas", 29, "Software Engineer");
   var person2 = createPerson("Greg", 27, "Doctor");
   ```

   工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎样知道一个对象的类型）。

4. **构造函数模式**

   像Object 和Array 这样的原生构造函数，在运行时会自动出现在执行环境中。此外，也可以创建自定义的构造函数，从而定义自定义对象类型的属性和方法。例如，可以使用构造函数模式将前面的例子重写如下。

   ```javascript
   function Person(name, age, job){
   	this.name = name;
   	this.age = age;
   	this.job = job;
   	this.sayName = function(){
   		alert(this.name);
   	};
   }
   var person1 = new Person("Nicholas", 29, "Software Engineer");
   var person2 = new Person("Greg", 27, "Doctor");
   ```

   提到检测对象类型，instanceof操作符要更可靠一些。我们在这个例子中创建的所有对象既是Object 的实例，同时也是Person的实例，这一点通过instanceof 操作符可以得到验证。

   ```javascript
   alert(person1 instanceof Object); //true
   alert(person1 instanceof Person); //true
   alert(person2 instanceof Object); //true
   alert(person2 instanceof Person); //true
   ```

   创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型；而这正是构造函数模式胜过工厂模式的地方。

   ​

   **将构造函数当作函数**

   构造函数与其他函数的唯一区别，就在于调用它们的方式不同。不过，构造函数毕竟也是函数，不存在定义构造函数的特殊语法。任何函数，只要通过new 操作符来调用，那它就可以作为构造函数；而任何函数，如果不通过new 操作符来调用，那它跟普通函数也不会有什么两样。例如，前面例子中定义的Person()函数可以通过下列任何一种方式来调用。

   ```javascript
   // 当作构造函数使用
   var person = new Person("Nicholas", 29, "Software Engineer");
   person.sayName(); //"Nicholas"
   // 作为普通函数调用
   Person("Greg", 27, "Doctor"); // 添加到window,当在全局作用域中调用一个函数时，this 对象总是指向                                  Global 对象（在浏览器中就是window 对象）
   window.sayName(); //"Greg"
   // 在另一个对象的作用域中调用
   var o = new Object();
   Person.call(o, "Kristen", 25, "Nurse");
   o.sayName(); //"Kristen"
   ```

   ​

   **构造函数的问题**

   使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。在前面的例子中，person1 和person2 都有一个名为sayName()的方法，但那两个方法不是同一个Function 的实例。不要忘了——ECMAScript 中的函数是对象，因此每定义一个函数，也就是实例化了一个对象。因此，不同实例上的同名函数是不相等的。

   然而，创建两个完成同样任务的Function 实例的确没有必要；况且有this 对象在，根本不用在
   执行代码前就把函数绑定到特定对象上面。因此，大可像下面这样，通过把函数定义转移到构造函数外
   部来解决这个问题。

   ```javascript
   function Person(name, age, job){
   	this.name = name;
   	this.age = age;
   	this.job = job;
   	this.sayName = sayName;
   }
   function sayName(){
   	alert(this.name);
   }
   var person1 = new Person("Nicholas", 29, "Software Engineer");
   var person2 = new Person("Greg", 27, "Doctor");
   ```

   在这个例子中，我们把sayName()函数的定义转移到了构造函数外部。而在构造函数内部，我们将sayName 属性设置成等于全局的sayName 函数。这样一来，由于sayName 包含的是一个指向函数的指针，因此person1 和person2 对象就共享了在全局作用域中定义的同一个sayName()函数。这样做确实解决了两个函数做同一件事的问题，可是新问题又来了：在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实。而更让人无法接受的是：如果对象需要定义很多方法，那么就要定义很多个全局函数，于是我们这个自定义的引用类型就丝毫没有封装性可言了。好在，这些问题可以通过使用原型模式来解决。

5. **原型模式**

   我们创建的每个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中，如下面的例子所示。

   ```javascript
   function Person(){
   }
   Person.prototype.name = "Nicholas";
   Person.prototype.age = 29;
   Person.prototype.job = "Software Engineer";
   Person.prototype.sayName = function(){
   	alert(this.name);
   };
   var person1 = new Person();
   person1.sayName(); //"Nicholas"
   var person2 = new Person();
   person2.sayName(); //"Nicholas"
   alert(person1.sayName == person2.sayName); //true
   ```

   虽然可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。如果我们在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。来看下面的例子。

   ```javascript
   function Person(){
   }
   Person.prototype.name = "Nicholas";
   Person.prototype.age = 29;
   Person.prototype.job = "Software Engineer";
   Person.prototype.sayName = function(){
   	alert(this.name);
   };
   var person1 = new Person();
   var person2 = new Person();
   person1.name = "Greg";
   alert(person1.name); //"Greg"——来自实例
   alert(person2.name); //"Nicholas"——来自原型
   ```

   当为对象实例添加一个属性时，这个属性就会屏蔽原型对象中保存的同名属性；换句话说，添加这个属性只会阻止我们访问原型中的那个属性，但不会修改那个属性。即使将这个属性设置为null，也只会在实例中设置这个属性，而不会恢复其指向原型的连接。不过，使用delete 操作符则可以完全删除实例属性，从而让我们能够重新访问原型中的属性，如下所示。

   ```javascript
   function Person(){
   }
   Person.prototype.name = "Nicholas";
   Person.prototype.age = 29;
   Person.prototype.job = "Software Engineer";
   Person.prototype.sayName = function(){
   	alert(this.name);
   };
   var person1 = new Person();
   var person2 = new Person();
   person1.name = "Greg";
   alert(person1.name); //"Greg"——来自实例
   alert(person2.name); //"Nicholas"——来自原型
   delete person1.name;
   alert(person1.name); //"Nicholas"——来自原型
   ```

   要取得对象上所有可枚举的实例属性，可以使用ECMAScript 5 的Object.keys()方法。这个方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组。例如：

   ```javascript
   function Person(){
   }
   Person.prototype.name = "Nicholas";
   Person.prototype.age = 29;
   Person.prototype.job = "Software Engineer";
   Person.prototype.sayName = function(){
   	alert(this.name);
   };
   var keys = Object.keys(Person.prototype);
   alert(keys); //"name,age,job,sayName"
   var p1 = new Person();
   p1.name = "Rob";
   p1.age = 31;
   var p1keys = Object.keys(p1);
   alert(p1keys); //"name,age"
   ```

   这里，变量keys 中将保存一个数组，数组中是字符串"name"、"age"、"job"和"sayName"。这个顺序也是它们在for-in 循环中出现的顺序。如果是通过Person 的实例调用，则Object.keys()返回的数组只包含"name"和"age"这两个实例属性。如果你想要得到所有实例属性，无论它是否可枚举，都可以使用Object.getOwnPropertyNames()方法。

   ```javascript
   var keys = Object.getOwnPropertyNames(Person.prototype);
   alert(keys); //"constructor,name,age,job,sayName"
   ```

   注意结果中包含了不可枚举的constructor 属性。Object.keys()和Object.getOwnProperty-Names()方法都可以用来替代for-in 循环。

   ​

   **更简单的原型语法**

   ```javascript
   function Person(){
   }
   Person.prototype = {
   	name : "Nicholas",
   	age : 29,
   	job: "Software Engineer",
   	sayName : function () {
   		alert(this.name);
   	}
   };
   ```

   尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是重写整个原型对象，那么情况就不一样了。我们知道，调用构造函数时会为实例添加一个指向最初原型的[[Prototype]]指针，而把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系。
   **请记住：实例中的指针仅指向原型，而不指向构造函数。**看下面的例子。

   ​

   **原型对象的问题**

   原型模式也不是没有缺点。首先，它省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值。虽然这会在某种程度上带来一些不方便，但还不是原型的最大问题。原型模式的最大问题是由其共享的本性所导致的。
   原型中所有属性是被很多实例共享的，这种共享对于函数非常合适。对于那些包含基本值的属性倒也说得过去，毕竟（如前面的例子所示），通过在实例上添加一个同名属性，可以隐藏原型中的对应属性。然而，对于包含引用类型值的属性来说，问题就比较突出了。来看下面的例子。

   ```javascript
   function Person(){
   }
   Person.prototype = {
   	constructor: Person,
   	name : "Nicholas",
   	age : 29,
   	job : "Software Engineer",
   	friends : ["Shelby", "Court"],
   	sayName : function () {
   		alert(this.name);
   	}
   };
   var person1 = new Person();
   var person2 = new Person();
   person1.friends.push("Van");
   alert(person1.friends); //"Shelby,Court,Van"
   alert(person2.friends); //"Shelby,Court,Van"
   alert(person1.friends === person2.friends); //true
   ```

   假如我们的初衷就是像这样在所有实例中共享一个数组，那么对这个结果我没有话可说。可是，实例一般都是要有属于自己的全部属性的。而这个问题正是我们**很少看到有人单独使用原型模式**的原因所在。

6. **组合使用构造函数模式和原型模式**

   创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式。**构造函数模式用于定义实例属性**，而**原型模式用于定义方法和共享的属性**。结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，**最大限度地节省了内存**。另外，这种混成模式还支持向构造函数传递参数；可谓是集两种模式之长。下面的代码重写了前面的例子。

   ```javascript
   function Person(name, age, job){
   	this.name = name;
   	this.age = age;
   	this.job = job;
   	this.friends = ["Shelby", "Court"];
   }
   Person.prototype = {
   	constructor : Person,
   	sayName : function(){
   		alert(this.name);
   	}
   }
   var person1 = new Person("Nicholas", 29, "Software Engineer");
   var person2 = new Person("Greg", 27, "Doctor");
   person1.friends.push("Van");
   alert(person1.friends); //"Shelby,Count,Van"
   alert(person2.friends); //"Shelby,Count"
   alert(person1.friends === person2.friends); //false
   alert(person1.sayName === person2.sayName); //true
   ```

   **这种构造函数与原型混成的模式，是目前在ECMAScript 中使用最广泛、认同度最高的一种创建自**
   **定义类型的方法。可以说，这是用来定义引用类型的一种默认模式。**

7. **动态原型模式**

   动态原型模式它把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。换句话说，可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。来看一个例子。

   ```javascript
   function Person(name, age, job){
   	//属性
   	this.name = name;
   	this.age = age;
   	this.job = job;
     //方法
   	if (typeof this.sayName != "function"){
   		Person.prototype.sayName = function(){
   			alert(this.name);
   		};
   	}
   }
   var friend = new Person("Nicholas", 29, "Software Engineer");
   friend.sayName();
   ```

   这里只在sayName()方法不存在的情况下，才会将它添加到原型中。这段代码只会在初次调用构造函数时才会执行。此后，原型已经完成初始化，不需要再做什么修改了。不过要记住，这里对原型所做的修改，能够立即在所有实例中得到反映。因此，这种方法确实可以说非常完美。


---

### 继承

1. #### 原型链

   ECMAScript 中描述了原型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。实现原型链有一种基本模式，其代码大致如下。

   ```javascript
   function SuperType(){
   	this.property = true;
   }
   SuperType.prototype.getSuperValue = function(){
   	return this.property;
   };
   function SubType(){
   	this.subproperty = false;
   }
   //继承了SuperType
   SubType.prototype = new SuperType();
   SubType.prototype.getSubValue = function (){
   	return this.subproperty;
   };
   var instance = new SubType();
   alert(instance.getSuperValue()); //true
   ```

   这个例子中的实例以及构造函数和原型之间的关系如图 所示。

   ​

   还有一点需要注意，即在通过原型链实现继承时，不能使用对象字面量创建原型方法。因为这样做就会重写原型链，如下面的例子所示。

   ```javascript
   function SuperType(){
   	this.property = true;
   }
   SuperType.prototype.getSuperValue = function(){
   	return this.property;
   };
   function SubType(){
   	this.subproperty = false;
   }
   //继承了SuperType
   SubType.prototype = new SuperType();
   //使用字面量添加新方法，会导致上一行代码无效
   SubType.prototype = {
   	getSubValue : function (){
   		return this.subproperty;
   	},
   	someOtherMethod : function (){
   		return false;
   	}
   };
   var instance = new SubType();
   alert(instance.getSuperValue()); //error!
   ```

   以上代码展示了刚刚把SuperType 的实例赋值给原型，紧接着又将原型替换成一个对象字面量而导致的问题。由于现在的原型包含的是一个Object 的实例，而非SuperType 的实例，因此我们设想中的原型链已经被切断——SubType 和SuperType 之间已经没有关系了。

   **原型链的问题**

   原型链虽然很强大，可以用它来实现继承，但它也存在一些问题。其中，最主要的问题来自包含引用类型值的原型。想必大家还记得，我们前面介绍过包含引用类型值的原型属性会被所有实例共享；而这也正是为什么要在构造函数中，而不是在原型对象中定义属性的原因。在通过原型来实现继承时，原型实际上会变成另一个类型的实例。于是，原先的实例属性也就顺理成章地变成了现在的原型属性了。下列代码可以用来说明这个问题。

   ```javascript
   function SuperType(){
   	this.colors = ["red", "blue", "green"];
   }
   function SubType(){
   }
   //继承了SuperType
   SubType.prototype = new SuperType();
   var instance1 = new SubType();
   instance1.colors.push("black");
   alert(instance1.colors); //"red,blue,green,black"
   var instance2 = new SubType();
   alert(instance2.colors); //"red,blue,green,black"
   ```

   这个例子中的SuperType 构造函数定义了一个colors 属性，该属性包含一个数组（引用类型值）。SuperType 的每个实例都会有各自包含自己数组的colors 属性。当SubType 通过原型链继承了SuperType 之后，SubType.prototype 就变成了SuperType 的一个实例，因此它也拥有了一个它自己的colors 属性——就跟专门创建了一个SubType.prototype.colors 属性一样。但结果是什么呢？结果是SubType 的所有实例都会共享这一个colors 属性。而我们对instance1.colors 的修改能够通过instance2.colors 反映出来，就已经充分证实了这一点。

   实践中很少会单独使用原型链。

2. #### 借用构造函数

   这种技术的基本思想相当简单，即在子类型构造函数的内部调用超类型构造函数。别忘了，函数只不过是在特定环境中执行代码的对象，因此通过使用apply()和call()方法也可以在（将来）新创建的对象上执行构造函数，如下所示：

   ```javascript
   function SuperType(){
   	this.colors = ["red", "blue", "green"];
   }
   function SubType(){
   	//继承了SuperType
   	SuperType.call(this);
   }
   var instance1 = new SubType();
   instance1.colors.push("black");
   alert(instance1.colors); //"red,blue,green,black"
   var instance2 = new SubType();
   alert(instance2.colors); //"red,blue,green"
   ```

   代码中加粗的那一行代码“借调”了超类型的构造函数。通过使用call()方法（或apply()方法也可以），我们实际上是在（未来将要）新创建的SubType 实例的环境下调用了SuperType 构造函数。这样一来，就会在新SubType 对象上执行SuperType()函数中定义的所有对象初始化代码。结果，SubType 的每个实例就都会具有自己的colors 属性的副本了。

   1. 传递参数

      相对于原型链而言，借用构造函数有一个很大的优势，即可以在子类型构造函数中向超类型构造函数传递参数。看下面这个例子。

      ```javascript
      function SuperType(name){
      	this.name = name;
      }
      function SubType(){
      	//继承了SuperType，同时还传递了参数
      	SuperType.call(this, "Nicholas");
      	//实例属性
      	this.age = 29;
      }
      var instance = new SubType();
      alert(instance.name); //"Nicholas";
      alert(instance.age); //29
      ```

   2. 借用构造函数的问题

      如果仅仅是借用构造函数，那么也将无法避免构造函数模式存在的问题——方法都在构造函数中定
      义，因此函数复用就无从谈起了。而且，在超类型的原型中定义的方法，对子类型而言也是不可见的，结
      果所有类型都只能使用构造函数模式。考虑到这些问题，借用构造函数的技术也是很少单独使用的。

3. #### 组合继承

   组合继承指的是将原型链和借用构造函数的技术组合到一块，从而发挥二者之长的一种继承模式。其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有它自己的属性。下面来看一个例子。

   ```javascript
   function SuperType(name){
   	this.name = name;
   	this.colors = ["red", "blue", "green"];
   }
   SuperType.prototype.sayName = function(){
   	alert(this.name);
   };
   function SubType(name, age){
   	//继承属性
   	SuperType.call(this, name);
   	this.age = age;
   }
   //继承方法
   SubType.prototype = new SuperType();
   SubType.prototype.constructor = SubType;
   SubType.prototype.sayAge = function(){
   	alert(this.age);
   };

   var instance1 = new SubType("Nicholas", 29);
   instance1.colors.push("black");
   alert(instance1.colors); //"red,blue,green,black"
   instance1.sayName(); //"Nicholas";
   instance1.sayAge(); //29

   var instance2 = new SubType("Greg", 27);
   alert(instance2.colors); //"red,blue,green"
   instance2.sayName(); //"Greg";
   instance2.sayAge(); //27
   ```

   在这个例子中，SuperType 构造函数定义了两个属性：name 和colors。SuperType 的原型定义了一个方法sayName()。SubType 构造函数在调用SuperType 构造函数时传入了name 参数，紧接着又定义了它自己的属性age。然后，将SuperType 的实例赋值给SubType 的原型，然后又在该新原型上定义了方法sayAge()。这样一来，就可以让两个不同的SubType 实例既分别拥有自己属性——包括colors 属性，又可以使用相同的方法了。**组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为JavaScript 中最常用的继**
   **承模式。**而且，instanceof 和isPrototypeOf()也能够用于识别基于组合继承创建的对象。

4. #### 原型式继承

   借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。为了达到这个目的，给出如下的函数：

   ```javascript
   function object(o){
   	function F(){}
   	F.prototype = o;
   	return new F();
   }
   ```

   来看下面的例子。

   ```javascript
   var person = {
   	name: "Nicholas",
   	friends: ["Shelby", "Court", "Van"]
   };
   var anotherPerson = object(person);
   anotherPerson.name = "Greg";
   anotherPerson.friends.push("Rob");

   var yetAnotherPerson = object(person);
   yetAnotherPerson.name = "Linda";
   yetAnotherPerson.friends.push("Barbie");
   alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
   ```

   ECMAScript 5 通过新增Object.create()方法规范化了原型式继承。这个方法接收两个参数：一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象。在传入一个参数的情况下，Object.create()与object()方法的行为相同。

   ```javascript
   var person = {
   	name: "Nicholas",
   	friends: ["Shelby", "Court", "Van"]
   };
   var anotherPerson = Object.create(person);
   anotherPerson.name = "Greg";
   anotherPerson.friends.push("Rob");

   var yetAnotherPerson = Object.create(person);
   yetAnotherPerson.name = "Linda";
   yetAnotherPerson.friends.push("Barbie");
   alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
   ```

   Object.create()方法的第二个参数与Object.defineProperties()方法的第二个参数格式相同：每个属性都是通过自己的描述符定义的。以这种方式指定的任何属性都会覆盖原型对象上的同名属性。例如：

   ```javascript
   var person = {
   	name: "Nicholas",
   	friends: ["Shelby", "Court", "Van"]
   };
   var anotherPerson = Object.create(person, {
   	name: {
   		value: "Greg"
   	}
   });
   alert(anotherPerson.name); //"Greg"
   ```

   在没有必要兴师动众地创建构造函数，而只想让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的。不过别忘了，包含引用类型值的属性始终都会共享相应的值，就像使用原型模式一样。

5. #### 寄生组合式继承

   所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其背
   后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型
   原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型
   的原型。寄生组合式继承的基本模式如下所示。

   ```javascript
   function inheritPrototype(subType, superType){
   	var prototype = object(superType.prototype); //创建对象
   	prototype.constructor = subType; //增强对象
   	subType.prototype = prototype; //指定对象
   }
   ```

   这个示例中的inheritPrototype()函数实现了寄生组合式继承的最简单形式。这个函数接收两个参数：子类型构造函数和超类型构造函数。在函数内部，第一步是创建超类型原型的一个副本。第二步是为创建的副本添加constructor 属性，从而弥补因重写原型而失去的默认的constructor 属性。最后一步，将新创建的对象（即副本）赋值给子类型的原型。这样，我们就可以用调用inherit-Prototype()函数的语句，去替换前面例子中为子类型原型赋值的语句了，例如：

   ```javascript
   function SuperType(name){
   	this.name = name;
   	this.colors = ["red", "blue", "green"];
   }
   SuperType.prototype.sayName = function(){
   	alert(this.name);
   };
   function SubType(name, age){
   	SuperType.call(this, name);
   	this.age = age;
   }
   inheritPrototype(SubType, SuperType);
   SubType.prototype.sayAge = function(){
   	alert(this.age);
   };
   ```

   这个例子的高效率体现在它只调用了一次SuperType 构造函数，并且因此避免了在SubType.
   prototype 上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用
   instanceof 和isPrototypeOf()。**开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。**

6. #### 










