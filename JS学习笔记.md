## JS学习笔记

断断续续加一些JS的知识或者用得着的技巧进来，方便查询。

1. **Math对象的min()和max()f方法**

   min()和max()方法用于确定一组数值中的最小值和最大值。这两个方法经常用于避免多余的循环和在if语句中确定一组数的最大值。

   ```javascript
   var max = Math.max(3,54,32,16);
   alert(max);		//54

   var min = Math.min(3,54,32,16);
   alert(min);		//3
   ```

   要找到数组中的最大值或最小值，可以像下面这样使用apply()方法。

   ```javascript
   var values = [1,2,3,4,5,6,7,8];
   var max = Math.max.apply(Math,values);
   ```

   这个技巧的关键是把Math对象作为apply()的第一个参数，从而正确地设置this值。然后，可以将任何数组作为第二个参数。

2. **Math对象的random()方法**

   Math.random()方法返回大于等于0小于1的一个随机数。套用下面的公式，就可以利用Math.random()从某个整数范围内随机选择一个值。

   ```javascript
   值 = Math.floor(Math.random()* 可能值的总数 + 第一个可能的值);
   ```

   举例来说，如果你想选择一个1到10之间的数值，可以像下面这样编写代码：

   ```javascript
   var num = Math.floor(Math.random() * 10 + 1);
   ```

   总共有10个可能的值（1到10），而第一个可能的值是1。而如果想要选择一个介于2到10之间的值，就应该将上面的代码改写成这样：

   ```javascript
   var num = Math.floor(Math.random() * 9 + 2);
   ```

   从2数到10要数9个数，因此可能值的总数就是9，而第一个可能的值就是2。多数情况下，其实可以通过一个函数来计算可能值的总数和第一个可能的值，例如：

   ```javascript
   function selectFrom(lowerValue, upperValue){
     var choices = upperValue - lowerValue + 1;
     return Math.floor(Math.random() * choices + lowerValue);
   }
   var num = selectFrom(2,10);
   alert(num);		//介于2和10之间（包括2和10）的一个数值
   ```

   利用这个函数，可以方便地从数组中随机取出一项，例如：

   ```javascript
   var colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"];
   var color = colors[selectFrom(0, colors.length-1)];
   alert(color);	//可能是数组中包含的任何一个字符串
   ```

   ​

3. ​ **递归**

   递归函数是在一个函数通过名字调用自身的情况下构成的，如下所示。

   ```javascript
   function factorial(num){
     if(num <= 1){
       return 1;
     }else{
       return num * factorial(num - 1);
     }
   }
   ```

   这是一个经典的递归阶乘函数。虽然这个函数表面看起来没什么问题，但下面的代码却可能导致它出错。

   ```javascript
   var anotherFactorial = factorial;
   factorial = null;
   alert(anotherFactorial(4));    //出错
   ```


   以上代码先把factorial()函数保存在变量anotherFactorial中，然后将factorial变量设置为null，结果指向原函数的引用只剩下一个。但在接下来调用anotherFactorial()时，由于必须执行factorial()，而factorial已经不再是函数，所以就会导致错误。在这种情况下，使用arguments.callee可以解决这个问题。

   我们知道，argumens.callee是一个指向正在执行的函数的指针，因此可以用它来实现对函数的递归调用，例如：

   ```javascript
   function factorial(num){
     if(num <= 1){
       return 1;
     }else{
       return num * arguments.callee(num - 1);
     }
   }
   ```

   通过使用arguments.callee代替函数名，可以确保无论怎样调用函数都不会出问题。因此，在编写递归函数时，使用arguments.callee总比使用函数名更保险。

   但在严格模式下，不能通过脚本访问arguments.callee，访问这个属性会导致错误。不过，可以使用**命名函数表达式**来达成相同的结果。例如：

   ```javascript
   var factorial = (function f(num){
     if(num <= 1){
       return 1;
     }else{
       return num * f(num - 1);
     }
   });
   ```

   以上代码创建了一个名为f()的命名函数表达式，然后将它赋值给变量factorial。即便把函数赋值给了另一个变量，函数的名字f仍然有效，所以递归调用照样能正确完成。这种方式在严格模式和非严格模式下都行得通。

4. **typeof操作符**

   因为ECMAScript是松散类型的，因此需要有一种手段来检测给定变量的数据类型——typeof就是提供这方面信息的操作符。对一个值使用typeof操作符可能返回下列某个字符串：

   - "undefined"——如果这个值未定义；
   - "boolean"——如果这个值是布尔值；
   - "string"——如果这个值是字符串；
   - "number"——如果这个值是数值；
   - "object"——如果这个值是对象或null；
   - "function"——如果这个值是函数；

   typeof是一个操作符而不是函数。有些时候，typeof操作符会返回一些令人迷惑但技术上却正确的值。比如，调用typeof null会返回“object”，因为特殊值null被认为是一个空的对象引用（因为从逻辑角度来看，null值表示一个空对象指针）。Safari 5 及之前版本、Chrome 7 及之前版本在对正则表达式调用typeof操作符时会返回"function"，而其他浏览器在这种情况下会返回"object"。

   在能力检测（又称特性检测）中，在可能的情况下，要尽量使用typeof进行能力检测。特别是，宿主对象没有义务让typeof返回合理的值。比如，在浏览器环境下测试任何对象的某个特性是否存在，要使用下面的函数：

   ```javascript
   //作者:Peter Michaux
   function isHostMethod(object, property){
     var t = typeof object[property];
     return t == 'function' || (!!(t == 'object' && object[property])) || t == 'unknown';
   }
   ```

   可以像下面这样使用这个函数：

   ```javascript
   var xhr = new ActiveXObject("Microsoft.XMLHttp");  
   result = isHostMethod(xhr, "open");  //true
   result = isHostMethod(xhr, "foo");   //false
   ```

5. **双逻辑操作符**

   JavaScript中经常使用双逻辑操作符。双逻辑非操作，会把一个值（数字，字符串…..）转换为布尔值。第一次逻辑非操作取反的布尔，第二次获得最初元素本身对应的布尔值。

   ```javascript
   alert(!!false);     //false

   alert(!!"blue");    //true

   alert(!!0);         //false

   alert(!!NaN)        //false

   alert(!!12345)     //true
   ```

   优点:双逻辑非操作符提高了程序执行的效率，比先存储后访问的效果更好。

   ```javascript
   var res1 = (0 && undefined);     //0
   var res2 = !!(0 && undefined);   //false

   /*
   对于 res1 我们每次还需要隐式转换成布尔值，if(Boolean(res1)),而 res2 已经是布尔值，所以使用双逻辑非操作符提高了程序执行的效率。
   这里说的先储存后的访问效果好，说的就是先储存布尔值。
   */
   ```

6. **eval()**

   ​

7. ​ **元素遍历**

   对于元素间的空格，IE9及之前的版本不会返回文本节点，而其他浏览器都会返回文本节点。这样，就导致了在使用childNodes和firstChild等属性时的行为不一致。为了弥补这一差异，Element Traversal规范新定义了一组属性。

   - childElementCount:返回子元素（不包括文本节点和注释）的个数。
   - firstElementChild:指向第一个子元素；firstChild的元素版。
   - lastElementChild:指向最后一个子元素；lastChild的元素版。
   - previousElementSibling:指向前一个同辈元素；previousSibling的元素版。
   - nextElementSibling:指向后一个同辈元素；nextSibling的元素版。

   支持的浏览器为DOM元素添加了这些属性，利用这些元素不必担心空白文本节点，从而可以更方便地查找DOM元素了。下面看一个例子。过去，要跨浏览器遍历某元素的所有子元素，需要像下面这样写代码。

   ```javascript
   var i, len, child = element.firstChild;
   while(child != element.lastChild){
     if(child.nodeType == 1){
       processChild(child);
     }
     child = child.nextSibling;
   }
   ```

   而使用Element Traversal新增的元素，代码会更简洁。

   ```javascript
   var i, len, child = element.firstElementChild;
   while(child != element.lastElementChild){
     processChild(child);
     child = child.nextElementSibling;
   }
   ```

   支持Element Traversal规范的浏览器有IE 9+, Firefox 3.5+, Safari 4+, Chrome 和Opera 10+。

8. **classList 属性**

   在操作类名时，需要通过className属性添加、删除和替换类名。因为className中是一个字符串，所以即使之修改字符串中的一部分，也必须每次都设置整个字符串的值。例如：

   ```html
    <div class = "bd user disabled">...</div>
   ```

   这个div元素一共有三个类名。要从中删除一个类名，需要把这三个类名拆开，删除不想要的那个，然后再把其他类名拼成一个新字符串。请看下面的例子。

   ```javascript
   //删除"user"类
   //首先，取得类名字符串并拆分成数组
   var classNames = div.className.split(/\s+/);
   //找到要删除的类名
   var pos = -1, i, len;
   for(i = 0, len = classNames.length; i < len; i++){
     if(classNames[i] == "user"){
       pos = i;
       break;
     }
   }
   //删除类名
   classNames.splice(i,1);
   //把剩下的类名拼成字符串并重新设置
   div.className = classNames.join(" ");
   ```

   HTML5新增了一种操作类名的方式，可以让操作更简单也更安全，那就是为所有元素添加classList属性。

   这样前面的多行代码用下面一行代码就可以代替了。

   ```javascript
   div.classList.remove("user");
   ```

   以上代码能够确保其他类名不受此次修改的影响。其他方法也能极大地减少类似基本操作的复杂性，如下面的例子所示。

   ```javascript
   //删除"disabled"类
   div.classList.remove("disabled");

   //添加"current"类
   div.classList.add("current");

   //切换"user"类
   div.classList.toggle("user");

   //确定元素中是否包含既定的类名
   if(div.classList.contains("bd") && !div.classList.contians("disabled")){
     //执行操作
   }

   //迭代类名
   for(var i = 0, len = div.classList.length; i < len; i++){
     doSomething(div.classList[i]);
   }
   ```

   有了classList属性，除非你需要全部删除所有类名，或者完全重写元素的class属性，否则也就用不到className属性了。支持classList属性的浏览器有Firefox 3.6+ 和Chrome。 

9. **取得元素的左偏移量和上偏移量**

   ```javascript
   function getElementLeft(element){
     var actualLeft = element.offsetLeft;
     var current = element.offsetParent;
     
     while(current != null){
       actualLeft += current.offsetLeft;
       current = current.offsetParent;
     }
     return actualLeft;
   }
   ```

   ```javascript
   function getElementTop(element){
     var actualTop = element.offsetTop;
     var current = element.offsetParent;
     
     while(current != null){
       actualTop += current.offsetTop;
       current = current.offsetParent;
     }
     return actualTop;
   }
   ```

   注意：所有这些偏移量属性都是只读的，而且每次访问它们都需要重新计算。因此，应该尽量避免重复访问这些属性；如果需要重复使用其中某些属性的值，可以将它们保存在局部变量中，以提高性能。

10. **确定浏览器视口大小**

  可以使用document.documentElement或document.body(在IE7之前的版本中)的clientWidth和clientHeight。

  ```javascript
  function getViewport(){
    if(document.compatMode == "BackCompat"){
      return {
        width: document.body.clientWidth,
        height: document.body.clientHeight
      };
    }else{
      return {
        width: document.documentElement.clientWidth,
        height: document.documentElement.clientHeight
      };
    }
  }
  ```

  这个函数首先检查document.compatMode属性，以确定浏览器是否运行在混杂模式。这个函数会返回一个对象，包含两个属性：width和height；表示浏览器视口（<html>或<body>）的大小。

11. **检测元素是否位于顶部，如果不是就将其滚回顶部**

    ```javascript
    function scrollToTop(element){
      if(element.scrollTop != 0){
        element.scrollTop = 0;
      }
    }
    ```

12. **事件流**

    事件流描述的是从页面中接收事件的顺序。

    1. 事件冒泡：

       IE的事件流叫做事件冒泡，即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接受，然后逐级向上传播到较为不具体的节点（文档）。

       所有现代浏览器都支持时间冒泡，但在具体实现上还是有一些差别。IE5.5及更早版本中的事件冒泡会跳过<html>元素（从<body>直接跳到document）。IE9、Firefox、Chrome和Safari则将事件一直冒泡到window对象。

    2. 事件捕获：

       事件捕获的思想是不太具体的节点应该更早接受到事件，而最具体的节点应该最后接收到事件。事件捕获的用以在于在事件到达预定目标之前捕获它。

       虽然事件捕获是Netscape Communicator 唯一支持的事件流模型，但IE9、Safari、Chrome、Opera和Firefox目前也都支持这种事件流模型。尽管“DOM2级事件”规范要求应该从document对象开始传播，但这些浏览器都是从window对象开始捕获事件的。

       由于老版本的浏览器不支持，因此很少有人使用事件捕获。建议读者放心地使用时间冒泡，在有特殊需要时再使用事件捕获。

    3. DOM事件流

       “DOM2级事件流规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和时间冒泡阶段。首先发生的是事件捕获，为截获事件提供了机会。然后是实际的目标接受到事件。最后一个阶段是时间冒泡阶段，可以在这个阶段对事件作出响应。

13. **事件处理程序**

    1. DOM0级事件处理程序

       ```javascript
       var btn = document.getElementById("myBtn");
       btn.onclick = function(){
         alert("Clicked");
       };
       ```

       使用DOM0级方法指定的事件处理程序被认为是元素的方法。因此，这时候的事件处理程序是在元素的作用域中运行；换句话说，程序中的this引用当前元素。

       ```javascript
       var btn = document.getElementByID("myBtn");
       btn.onclick = function(){
         alert(this.id);
       };//"myBtn"
       ```

       实际上可以在事件处理程序中通过this访问元素的任何属性和方法。以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理。

       也可以删除通过DOM0级方法指定的事件处理程序，只要像下面这样将事件处理程序属性的值设置为null即可：

       ```javascript
       btn.onclick = null;
       ```

       这种方式也可以删除通过HTML指定的事件处理程序。

    2. DOM2级事件处理程序

       “DOM2级事件”定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener()和removeEventListener()。它们接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值如果是true，表示在捕获阶段调用事件处理程序；如果是false，表示在冒泡阶段调用事件处理程序。

       ```javascript
       var　btn = document.getElementById("myBtn");
       btn.addEventListener("click",function(){
         alert(this.id);
       },false);
       ```

       与DOM0级方法一样，这里添加的事件处理程序也是在其依附的元素的作用域中运行。使用DOM2级方法添加事件处理程序的主要好处是可以添加多个事件处理程序。

       ```javascript
       var btn = document.getElementById("myBtn");
       btn.addEventListener("click",function(){
         alert(this.id);
       },false);
       btn.addEventListener("click",function(){
         alert("Hello World!");
       },false);
       ```

       这里为按钮添加了两个事件处理程序。这两个事件处理程序会按照添加它们的顺序触发，因此首先会显示元素ID，其次会显示“Hello World!”消息。

       通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除；移除是传入的参数与添加事件处理程序时使用的参数相同。这也意味着通过addEventListener()添加的匿名函数将无法移除，如下面的例子所示。

       ```javascript
       var btn = document.getElementById("myBtn");
       btn.addEventListener("click",function(){
         alert(this.id);
       },false);
       btn.removeEventListener("clici",function(){//没有用
         alert(this.id);
       },false);
       ```

       removeEventListener()中第二个参数与传入addEventListener()中的那一个是完全不同的函数。而传入removeEventListener()中的事件处理程序函数必须与传入addEventListener()中的相同。如下面的例子所示：

       ```javascript
       var btn = document.getElementById("myBtn");
       var handler = function(){
         alert(this.id);
       };
       btn.addEventListener("click",handler,false);
       btn.removeEventListener("clici",handler,false);//有效
       ```

       大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览器。最好只在需要在事件到达目标之前截获它的时候将事件处理程序添加到捕获阶段。如果不是特别需要，不建议在事件捕获阶段注册事件处理程序。

    3. IE事件处理程序

       IE实现了与DOM中类似的两个方法：attachEvent()和detachEvent()。这两个方法接受相同的两个参数：事件处理程序名称和事件处理程序函数。由于IE8及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。

       ```javascript
       var btn = document.getElementById("myBtn");
       btn.attachEvent("onclick",function(){
         alert("Clicked");
       });
       ```

       注意，attachEvent()的第一个参数是“onclick”，而非DOM的addEventListener()方法中的“click”。

       在IE中使用attachEvent()与使用DOM0级方法的主要区别在于事件处理程序的作用域。在使用DOM0级方法的情况下，事件处理程序会在其所属元素的作用域内运行；在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中运行，因此this等于window。

       ```javascript
       var btn = document.getElementById("myBtn");
       btn.attachEvent("onclick",function(){
         alert(this == window);//true
       });
       ```

       在编写跨浏览器的代码时，牢记这一区别很重要。

       与addEventListener()类似，attachEvent()方法也可以用来为一个元素添加多个事件处理程序。来看下面的例子。

       ```javascript
       var btn = document.getElementById("myBtn");
       btn.attachEvent("onclick",function(){
         alert("Clicked");
       });
       btn.attacnEvent("onclick",function(){
         alert("Hello World!");
       });
       ```

       这里调用了两次attachEvent()，为同一个按钮添加了两个不同的事件处理程序。不过，与DOM方法不同的是，这些事件处理程序不是以添加它们的顺序执行，而是以相反的顺序被触发。单击这个例子中的按钮，首先看到的是“Hello World!”，然后才是“Clicked”。

       使用attachEvent()添加的事件可以通过detachEvent()来移除，条件是必须提供相同的参数。与DOM方法一样，这也意味着添加的匿名函数将不能被移除。

       ```javascript
       var btn = document.getElementById("myBtn");
       var handler = function(){
         alert("Clicked");
       };
       btn.attacnEvent("onclick",handler);
       btn.detacnEvent("onclick",handler);
       ```

       支持IE事件处理程序的浏览器有IE 和Opera。

    4. 跨浏览器的事件处理程序

       ```javascript
       var EventUtil = {
         addHandler: function(element, type, handler){
           if(element.addEventListener){
             element.addEventListener(type, handler, false);
           }else if(element.attachEvent){
             element.attachEvent("on" + type, handler);
           }else{
             element["on" + type] = handler;
           }
         },
          removeHandler: function(element, type, handler){
           if(element.removeEventListener){
             element.removeEventListener(type, handler, false);
           }else if(element.detachEvent){
             element.detachEvent("on" + type, handler);
           }else{
             element["on" + type] = null;
           }
         }
       };
       ```

       可以像下面这样使用EventUtil对象：

       ```javascript
       var btn = document.getElementById("myBtn");
       var handler = function(){
         alert("Clicked");
       };
       EventUtil.addHandler(btn, "click", handler);
       //这里省略了其他代码
       EventUtil.removeHandler(btn, "click", handler);
       ```

       addHandler()和removeHandler()没有考虑到所有的浏览器问题，例如在IE中的作用域问题。不过，使用它们添加和移除事件处理程序还是足够了。此外还要注意，DOM0级对每个事件只支持一个事件处理程序。好在，只支持DOM0级的浏览器已经没有那么多了，因此这对你而言应该不是什么问题。

14. **跨浏览器的事件对象**

    在13.4的基础上将EventUtil对象加以增强，添加一些方法来求同存异，解决DOM和IE中对象不同的问题。

    ```javascript
    var EventUtil = {
      addHandler: function(element, type, handler){
        if(element.addEventListener){
          element.addEventListener(type, handler, false);
        }else if(element.attachEvent){
          element.attachEvent("on" + type, handler);
        }else{
          element["on" + type] = handler;
        }
      },
      getEvent: function(event){
        return event ? event : window.event;
      },
      getTarget: function(event){
        return event.target || event.srcElement;
      },
      preventDefault: function(event){
        if(event.preventDefault){
          event.preventDefault();
        }else{
          event.returnValue = false;
        }
      },
      removeHandler: function(element, type, handler){
        if(element.removeEventListener){
          element.removeEventListener(type, handler, false);
        }else if(element.detachEvent){
          element.detachEvent("on" + type, handler);
        }else{
          element["on" + type] = null;
        }
      },
      stopPropagation: function(event){
        if(event.stopPropagation){
          event.stopPropagation();
        }else{
          event.cancelBubble = true;
        }
      }
    };
    ```

    注意：在使用新增的4个方法的时候，必须假设有一个事件对象传入到事件处理程序中，而且要把该变量传给这个方法，如下所示：

    ```javascript
    btn.onclick = function(event){
      event = EventUtil.getEvent(event);
    };
    ```

15. **对EventUtil对象的进一步加强**

    ```javascript
    var EventUtil = {
     //省略了其他代码
      
      //跨浏览器取得相关元素的方法
      getRelatedTarget: function(event){
        if(event.relatedTarget){
          return event.relatedTarget;
        }else if(event.toElement){
          return event.toElement;
        }else if(event.fromElement){
          return event.fromElement;
        }else{
          return null;
        }
      },
      
      //因为DOM和IE两种模型下的button属性有很大的差异，而且同名，无法单独使用能力检测来确定差异
      getButton: function(event){
        if(document.implementation.hasFeature("MouseEvents", "2.0")){
          return event.button;
        }else{
          switch(event.button){
            case 0:
            case 1:
            case 3:
            case 5:
            case 7:
              return 0;
            case 2:
            case 6:
              return 2;
            case 4:
              return 1;
          }
        }
      },
      
      //滚轮滚动时获得wheelDelta或者detail的值，不同浏览器有区别
      getWheelDelta: function(event){
        if(event.wheelDelta){
          return (client.engine.opera && client.engine.opera < 9.5 ? -event.wheelDelta : event.wheelDelta);
        }else{
          return -event.detail * 40;
        }
      },
      
      //要想以跨浏览器的方式取得字符编码，必须首先检测charCode属性是否可用，如果不可用则使用keyCode
      getCharCode: function(event){
        if(typeof event.charCode == "number"){
          return event.charCode;
        }else{
          return event.keyCode;
        }
      },
      
      //从剪切板中获得数据
      getClipboardText: function(event){
        var clipboardData = (event.clipboardData || window.clipboardData);
        return clipboardData.getData("text");
      },
      
      //设置要放到剪切板中的文本
      setClipboardText: function(event, value){
        if(event.clipboardData){
          return event.clipboardData.setData("text/plain", value);
        }else if(window.clipboardData){
          return window.clipboardData.setData("text", value);
        }
      }
      //省略了其他代码
    };
    ```

16. **​内存和性能**

    JavaScript中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能。导致这一问题的原因是多方面的。

    * 首先，每个函数都是对象，都会占用内存；内存中的对象越多，性能就越差。
    * 其次，必须事先指定所有事件处理程序而导致的DOM访问次数，会延迟整个页面的交互就绪时间。

    事实上，从如何利用好时间处理程序的角度出发，还是有一些方法能够提升性能的。

    1. **事件委托**

       对“事件处理程序过多”问题的解决方案就是事件委托。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。例如，click事件会一直冒泡到document层次。也就是说，我们可以为整个页面指定一个onclick事件处理程序，而不必给每个可单击的元素分别添加事件处理程序。以下面的HTML代码为例。

       ```html
       <uln id = "myLinks">
         <li id = "goSomewhere">Go somewhere</li>
         <li id = "doSomething">Do something</li>
         <li id = "sayHi">Say hi</li>
       </ul>
       ```

       按照传统的做法，需要向下面这样为它们添加3个事件梳理程序。

       ```javascript
       var item1 = document.getElementById("goSomewhere");
       var item2 = document.getElementById("doSomething");
       var item3 = document.getElementById("sayHi");

       EventUtil.addHandler(item1, "click", function(event){
         location.href = "http://www.wrox.com";
       });

       EventUtil.addHandler(item2, "click", function(event){
         document.title = "I changed the document's title";
       });

       EventUtil.addHandler(item3, "click", function(event){
         alert("hi");
       });
       ```

       如果在一个复杂的Web应用程序中，对所有可单击的元素都采用这种方式，那么结果就会有数不清的代码用于添加事件处理程序。此时，可以利用事件委托技术解决这个问题。使用事件委托，只需在DOM树中尽量最高的层次上添加一个事件处理程序，如下面的例子所示：

       ```javascript
       var list = document.getElementById("myLinks");
       EventUtil.addHandler(list, "click", function(event){
         event = EventUtil.getEvent(event);
         var target = EventUtil.getTarget(event);
         switch(target.id){
           case "doSomething":
             document.title = "I changed the document's title";
             break;
           case "goSomewhere":
              location.href = "http://www.wrox.com";
              break;
           case "sayHi":
             alert("hi");
             break;
         }
       });
       ```

       在这段代码里，我们使用事件委托只为<ul>元素添加了一个onclick事件处理程序。由于所有列表项都是这个元素的子节点，而且它们的事件会冒泡，所以单击事件最终会被这个函数处理。与前面未使用事件委托的代码一比，会发现这段代码的事前消耗更低，因为只取得了一个DOM元素，只添加了一个事件处理程序。这种技术需要占用的内存更少。所有用到按钮的事件(多数鼠标事件和键盘事件)都适合采用事件委托技术，包括click、mousedown、mouseup、keydown、keyup和keypress。

    2. **移除事件处理程序**

       每当将事件处理程序指定给元素时，运行中的浏览器代码与支持页面交互的JavaScript代码之间就会建立一个连接。这种连接越多，页面执行起来就越慢。如前所述，可以采用事件委托技术，限制建立的连接数量。另外，在不需要的时候移除事件处理程序，也是解决这个问题的一种方案。内存中留有那些过时不用的"空事件处理程序"，也是造成Web应用程序内存与性能问题的主要原因。

       在两种情况下，可能会造成上述问题。第一种情况就是从文档中移除带有事件处理程序的元素时。这可能是通过纯粹的DOM操作，例如使用removeChlid()和replaceChild()方法，但更多地是发生在使用innerHTML替换页面中某一部分的时候。如果带有事件处理程序的元素被innerHTML删除了，那么原来添加到元素中的事件处理程序极有可能无法被当作垃圾回收。来看下面的例子。

       ```html
       <div id = "myDiv">
       	<input type = "button" value = "Click Me" id = "myBtn">  
       </div>
       <script type = "text/javascript">
           var btn = document.getElementById("myBtn");
           btn.onclick = function(){
             //先执行某些操作
             document.getElementById("myDiv").innerHTML = "Processing...";//麻烦了
           }
       </script>
       ```

       问题在于，当按钮被从页面移除时，它还带着一个事件处理程序呢。在<div>元素上设置innerHTML可以把按钮移走，但事件处理程序仍然与按钮保持着引用关系。有的浏览器（尤其是IE）在这种情况下不会作出恰当的处理，它们很有可能会将对元素和对事件处理程序的引用都保存在内存中。如果你知道某个元素即将被移除，那么最好手工移除事件处理程序，如下面例子所示：

       ```html
       <div id = "myDiv">
       	<input type = "button" value = "Click Me" id = "myBtn">  
       </div>
       <script type = "text/javascript">
           var btn = document.getElementById("myBtn");
           btn.onclick = function(){
             //先执行某些操作
             btn.onclick = null;//移除事件处理程序
             document.getElementById("myDiv").innerHTML = "Processing...";//麻烦了
           }
       </script>
       ```

       这样就确保了内存可以被再次引用，而从DOM中移除按钮也做到了干净利索。

       导致“空事件处理程序”的另一种情况，就是卸载页面的时候。毫不奇怪，IE8及更早版本在这种情况下依然是问题最多的浏览器，尽管其他浏览器或多或少也有类似的问题。如果在页面被卸载之前没有清理干净事件处理程序，那它们就会滞留在内存中。每次加载完页面再卸载页面时（可能是在两个页面间来回切换，也可以是单击了“刷新”按钮），内存中滞留的对象数目就会增加，因为事件处理程序占用的内存并没有被释放。

       一般来说，最好的做法是在页面卸载之前，先通过onunload事件处理程序移除所有的事件处理程序。在此，事件委托技术再次表现出它的优势——需要跟踪的事件处理程序越少，移除它们就越容易。对这种类似撤销的操作，我们可以把它想象成：只要是通过onload事件处理程序添加的东西，最后都要通过onunload事件处理程序将它们移除。​

17. **回调：**

    我们给某个方法传递了一个函数，这个方法在有相应事件发生时调用这个函数来进行回调。

18. **表单提交**

    提交表单时可能出现的最大问题，就是重复提交表单。在第一次提交表单后，如果长时间没有反应，用户可能会变得不耐烦。这时候，他们也许会反复单击提交按钮。结果往往很麻烦（因为服务器要处理处理重复的请求），或者会造成错误（如果用户是下订单，那么可能会多订好几份）。解决这一问题的办法有两个：在第一次提交表单后就禁用提交按钮，或者利用onsubmit事件处理程序取消后续的表单提交操作。

    只要监听submit事件，并在该事件发生时禁用提交按钮即可。

    ```javascript
    //避免多次提交表单
    EventUtil.addHandler(form, "submit", function(event){
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
      
      //取得提交按钮
      var btn = target.elements["submit-btn"];
      
      //禁用它
      btn.disabled = true;
    });
    ```

    这种方式不适合表单中不包含提交按钮的情况。只有在包含提交按钮的情况下，才有可能触发表单的submit事件。

19. **媒体元素**

    因为并非所有浏览器都支持所有媒体格式，所以可以指定多个不同的媒体来源。为此，不用在标签中指定src属性，而是要像下面这样使用一或多个<source>元素。

    ```html
    <!--嵌入视频-->
    <video id = "myVideo">
      <source src="conference.webm" type="video/webm; codecs='vp8, vorbis'">
      <source src="conference.ogv" type="video/ogg; codecs='theora, vorbis'">
      <source src="conference.mpg">
      Video player not available.
    </video>

    <!--嵌入音频-->
    <audio id = "myVideo">
      <source src="song.ogg" type="audio/ogg">
      <source src="song.mp3" type="audio/mpeg">
      Audio player not available.
    </audio>
    ```

    不同浏览器支持不同的编解码器，因此一般来说指定多种格式的媒体来源是必须的。

20. **JSON**

    JSON是一个轻量级的数据格式，可以简化表示复杂数据结构的工作量。JSON使用JavaScript语法的子集表示对象、数组、字符串、数值、布尔值和null。

    关于JSON，最重要的是要理解它是一种数据格式，不是编程语言。虽然具有相同的语法形式，但JSON并不从属于JavaScript。而且，并不是只有JavaScript才使用JSON，毕竟JSON只是一种数据格式。JSON不支持变量、函数或对象实例，它就是一种表示结构化数据的格式。

    JSON字符串与JavaScript字符串的最大区别在于，JSON字符串必须使用双引号（单引号会导致语法错误）。

    ECMAScript 5定义了一个原生的JSON对象，可以用来将对象序列化为JSON字符串或者将JSON数据解析为JavaScript对象。JSON.stringify()和JSON.parse()方法分别用来实现上述两项功能。这两个方法都有一些选项，通过它们可以改变过滤的方式，或者改变序列化的过程。

21. **JS数组的splice()方法**

    splice()方法恐怕要算是最强大的数组方法了，它有很多种用法。它的主要用途是向数组的中部插入值。使用方法如下：

    1. 删除：可以删除任意数量的项，只需指定2个参数：要删除的第一项位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项。

    2. 插入：可以向指定位置插入任意数量的项，只需提供3 个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0,"red","green")会从当前数组的位置2 开始插入字符串"red"和"green"。

    3. 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,"red","green")会删除当前数组位置2 的项，然后再从位置2 开始插入字符串"red"和"green"。
       splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项（如果没有删除任何项，则返回一个空数组）。下面的代码展示了上述3 种使用splice()方法的方式。

       ```javascript
       var colors = ["red", "green", "blue"];
       var removed = colors.splice(0,1); // 删除第一项
       alert(colors); // green,blue
       alert(removed); // red，返回的数组中只包含一项

       removed = colors.splice(1, 0, "yellow", "orange"); // 从位置1 开始插入两项
       alert(colors); // green,yellow,orange,blue
       alert(removed); // 返回的是一个空数组

       removed = colors.splice(1, 1, "red", "purple"); // 插入两项，删除一项
       alert(colors); // green,red,purple,orange,blue
       alert(removed); // yellow，返回的数组中只包含一项
       ```

       上面的例子首先定义了一个包含3 项的数组colors。第一次调用splice()方法只是删除了这个数组的第一项，之后colors 还包含"green"和"blue"两项。第二次调用splice()方法时在位置1 插入了两项，结果colors 中包含"green"、"yellow"、"orange"和"blue"。这一次操作没有删除项，因此返回了一个空数组。最后一次调用splice()方法删除了位置1 处的一项，然后又插入了"red"和"purple"。在完成以上操作之后，数组colors 中包含的是"green"、"red"、"purple"、"orange"和"blue"。

22. **跨域源资源共享**

    通过XHR（XMLHttpRequest）实现Ajax通信的一个主要限制，来源于跨域安全策略。默认情况下，XHR对象只能访问与包含它的页面位于同一个域中的资源。这种安全策略可以预防某些恶意行为。

    CORS(Cross-Origin Resource Sharing，跨域源资源共享)，其背后的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。

    **跨浏览器的CORS：**

    检测XHR是否支持CORS的最简单方式，就是检查是否存在withCredentials属性。再结合检测XDomainRequest对象是否存在，就可以兼顾所有浏览器了。

    ```javascript
    function createCORSRequest(method,url){
      var xhr = new XMLHttpRequest();
      if("withCredentials" in xhr){
        xhr.open(method, url, true);
      }else if(typeof XDomainRequest != "undefined"){
        xhr = new XDomainRequest();
        xhr.open(method,url);
      }else{
        xhr = null;
      }
      return xhr;
    }
    var request = createCORSRequest("get","http://www.somewhere-else.com/page/");
    if(request){
      request.onload = function(){
        //对request.responseText 进行处理
      };
      request.send();
    }
    ```

    **其他跨域技术：**

    1. 图像Ping: 只能用于浏览器与服务器间的单向通信。
    2. JSONP：是JSON with padding(填充式JSON或参数式JSON)的缩写，是应用JSON的一种新方法，在Web服务中非常流行。看起来与JSON差不多，只不过是被包含在函数调用中的JSON。它由两部分组成：回调函数和数据。优点在于能够直接访问响应文本，支持在浏览器与服务器之间双向通信。
    3. Comet：Ajax是是一种从页面向服务器请求数据的技术，而Comet则是一种服务器向页面推送数据的技术。有两种实现Comet的方式：长轮询和流。
    4. SSE（服务器发送事件）：是围绕只读Comet交互推出的API或者模式。
    5. Web Sockets: 其目标是在一个单独的持久连接上提供全双工、双向通信。由于传递的数据包很小，因此Web Sockets非常适合移动应用。同源策略对Web Sockets不适用，因此可以通过它打开任何站点的连接。

23. **减少全局变量污染**

    JavaScirpt 可以很随意地定义全局变量来容纳你的应用的所有资源。遗憾的是，全局变量削弱了程序的灵活性，应该避免使用。

    1. 最小化使用全局变量的方法之一是为你的应用只创建一个唯一的全局变量。

       ```javascript
       var MYAPP = {};
       ```

       该变量此时变成了你的应用的容器：

       ```javascript
       MYAPP.stooge = {
           "first-name": "Joe",
           "last-name": "Howard"
       };
       MYAPP.flight = {
         airline: "Oceanic",
         number: 815,
         departure: {
             IATA: "SYD",
             time: "2004-09-22 14:55",
             city: "Sydney"
         },
         arrival:{
             IATA: "LAX",
             time: "2004-09-23 10:42",
             city: "Los Angeles"
         }
       };
       ```

       只要把全局性的资源都纳入一个名称空间之下，你的程序与其他应用程序、组件或类库之间发生冲突的可能性就会显著降低。你的程序也会变得更容易阅读，因为很明显，MYAPP.stooge指向的是顶层结构。

    2. 闭包：

       ​

    3. ​

    ​

24. **​web会话管理**

http是无状态的，一次请求结束，连接断开，下次服务器再收到请求，它就不知道这个请求是哪个用户发过来的。当然它知道是哪个客户端地址发过来的，但是对于我们的应用来说，我们是靠用户来管理，而不是靠客户端。所以对我们的应用而言，它是需要有状态管理的，以便服务端能够准确的知道http请求是哪个用户发起的，从而判断他是否有权限继续这个请求。这个过程就是常说的会话管理。它也可以简单理解为一个用户从登录到退出应用的一段期间。

25. **cookie、localStorage、sessionStorage的概念和区别**

    基本概念

    ##### Cookie

    Cookie 是小甜饼的意思。顾名思义，cookie 确实非常小，它的大小限制为4KB左右，是网景公司的前雇员 Lou Montulli 在1993年3月的发明。它的主要用途有保存登录信息，比如你登录某个网站市场可以看到“记住密码”，这通常就是通过在 Cookie 中存入一段辨别用户身份的数据来实现的。

    ##### localStorage

    localStorage 是 HTML5 标准中新加入的技术，它并不是什么划时代的新东西。早在 IE 6 时代，就有一个叫 userData 的东西用于本地存储，而当时考虑到浏览器兼容性，更通用的方案是使用 Flash。而如今，localStorage 被大多数浏览器所支持，如果你的网站需要支持 IE6+，那以 userData 作为你的 polyfill 的方案是种不错的选择。

    | 特性             | Chrome | Firefox (Gecko) | Internet Explorer | Opera | Safari (WebKit) |
    | -------------- | ------ | --------------- | ----------------- | ----- | --------------- |
    | localStorage   | 4      | 3.5             | 8                 | 10.50 | 4               |
    | sessionStorage | 5      | 2               | 8                 | 10.50 | 4               |

    ##### sessionStorage

    sessionStorage 与 localStorage 的接口类似，但保存数据的生命周期与 localStorage 不同。做过后端开发的同学应该知道 Session 这个词的意思，直译过来是“会话”。而 sessionStorage 是一个前端的概念，它只是可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。但当页面关闭后，sessionStorage 中的数据就会被清空。

    ##### 三者的异同

    | 特性      | Cookie                                | localStorage                        | sessionStorage                      |
    | ------- | ------------------------------------- | ----------------------------------- | ----------------------------------- |
    | 数据的生命期  | 可设置失效时间，默认是关闭浏览器后失效                   | 除非被清除，否则永久保存                        | 仅在当前会话下有效，关闭页面或浏览器后被清除              |
    | 存放数据大小  | 4K左右                                  | 一般为5MB                              | 一般为5MB                              |
    | 与服务器端通信 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信           | 仅在客户端（即浏览器）中保存，不参与和服务器的通信           |
    | 易用性     | 需要程序员自己封装，源生的Cookie接口不友好              | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |

    ##### 应用场景

    有了对上面这些差别的直观理解，我们就可以讨论三者的应用场景了。

    因为考虑到每个 HTTP 请求都会带着 Cookie 的信息，所以 Cookie 当然是能精简就精简啦，比较常用的一个应用场景就是判断用户是否登录。针对登录过的用户，服务器端会在他登录时往 Cookie 中插入一段加密过的唯一辨识单一用户的辨识码，下次只要读取这个值就可以判断当前用户是否登录啦。曾经还使用 Cookie 来保存用户在电商网站的购物车信息，如今有了 localStorage，似乎在这个方面也可以给 Cookie 放个假了~

    而另一方面 localStorage 接替了 Cookie 管理购物车的工作，同时也能胜任其他一些工作。比如HTML5游戏通常会产生一些本地数据，localStorage 也是非常适用的。如果遇到一些内容特别多的表单，为了优化用户体验，我们可能要把表单页面拆分成多个子页面，然后按步骤引导用户填写。这时候 sessionStorage 的作用就发挥出来了。

    ##### 安全性的考虑

    需要注意的是，不是什么数据都适合放在 Cookie、localStorage 和 sessionStorage 中的。使用它们的时候，需要时刻注意是否有代码存在 XSS 注入的风险。因为只要打开控制台，你就随意修改它们的值，也就是说如果你的网站中有 XSS 的风险，它们就能对你的 localStorage 肆意妄为。所以千万不要用它们存储你系统中的敏感数据。

26. **安全的类型检测**

    在任何值上调用object原生的toString()方法，都会返回一个[object  NativeConstructorName]格式的字符串。每个类在内部都有一个[][][[Class]]属性，这个属性就指定了上述字符串中的构造函数名。

    ```javascript
    如果value是一个定义的数组
    alert(Object.prototype.toString.call(value)); //"[object Array]"
    ```

    由于原生数组的构造函数名与全局作用域无关，因此使用toString()就能保证返回一致的值，利用这一点可以创建如下 的函数：

    ```javascript
    function isArray(value){
      return Object.prototype.toString.call(value) == "[object Array]";
    }
    function isFunction(value){
      return Object.prototype.toString.call(value) == "[object Function]"
    }
    function isRegExp(value){
      return Object.prototype.toString.call(value) == "[object RegExp]"
    }
    //以上都是用来测试某个值是不是原生数组，函数或正则表达式
    ```

    这一技巧也广泛应用于检测原生JSON对象。Object的toString()方法不能检测非原生构造函数的构造函数名。因此，开发人员定义的任何构造函数都将返回[object Object]。

    在Web开发中能够区分原生与非原生JavaScript对象非常重要。只有这样才能确切知道某个对象到底有哪些功能。

27. **内存与性能**

    由于事件处理程序可以为现代Web 应用程序提供交互能力，因此许多开发人员会不分青红皂白地
    向页面中添加大量的处理程序。在创建GUI 的语言（如C#）中，为GUI 中的每个按钮添加一个onclick
    事件处理程序是司空见惯的事，而且这样做也不会导致什么问题。**可是在JavaScript 中，添加到页面上**
    **的事件处理程序数量将直接关系到页面的整体运行性能**。导致这一问题的原因是多方面的。首先，**每个**
    **函数都是对象，都会占用内存；内存中的对象越多，性能就越差。其次，必须事先指定所有事件处理程**
    **序而导致的DOM访问次数，会延迟整个页面的交互就绪时间**。事实上，从如何利用好事件处理程序的
    角度出发，还是有一些方法能够提升性能的。

    ​

    **事件委托：**

    对“事件处理程序过多”问题的解决方案就是事件委托。事件委托利用了事件冒泡，只指定一个事
    件处理程序，就可以管理某一类型的所有事件。例如，click 事件会一直冒泡到document 层次。也就
    是说，我们可以为整个页面指定一个onclick 事件处理程序，而不必给每个可单击的元素分别添加事
    件处理程序。以下面的HTML 代码为例。


    ```html
    <ul id="myLinks">
      <li id="goSomewhere">Go somewhere</li>
      <li id="doSomething">Do something</li>
      <li id="sayHi">Say hi</li>
    </ul>	
    ```

    其中包含3 个被单击后会执行操作的列表项。按照传统的做法，需要像下面这样为它们添加3 个事
    件处理程序。

    ```javascript
    var item1 = document.getElementById("goSomewhere");
    var item2 = document.getElementById("doSomething");
    var item3 = document.getElementById("sayHi");
    EventUtil.addHandler(item1, "click", function(event){
    	location.href = "http://www.wrox.com";
    });
    EventUtil.addHandler(item2, "click", function(event){
    	document.title = "I changed the document's title";
    });
    EventUtil.addHandler(item3, "click", function(event){
    	alert("hi");
    });
    ```

    如果在一个复杂的Web 应用程序中，对所有可单击的元素都采用这种方式，那么结果就会有数不
    清的代码用于添加事件处理程序。此时，可以利用事件委托技术解决这个问题。**使用事件委托，只需在**
    **DOM 树中尽量最高的层次上添加一个事件处理程序**，如下面的例子所示。

    ```javascript
    var list = document.getElementById("myLinks");
    EventUtil.addHandler(list, "click", function(event){
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
      switch(target.id){
        case "doSomething":
          document.title = "I changed the document's title";
          break;
        case "goSomewhere":
          location.href = "http://www.wrox.com";
          break;
        case "sayHi":
            alert("hi");
            break;
      }
    });
    ```

    在这段代码里，我们使用事件委托只为<ul>元素添加了一个onclick 事件处理程序。由于所有列
    表项都是这个元素的子节点，而且它们的事件会冒泡，所以单击事件最终会被这个函数处理。我们知道，
    事件目标是被单击的列表项，故而可以通过检测id 属性来决定采取适当的操作。与前面未使用事件委
    托的代码比一比，会发现这段代码的事前消耗更低，因为只取得了一个DOM 元素，只添加了一个事件
    处理程序。虽然对用户来说最终的结果相同，但这种技术需要占用的内存更少。所有用到按钮的事件（多
    数鼠标事件和键盘事件）都适合采用事件委托技术。
    如果可行的话，也可以考虑为document 对象添加一个事件处理程序，用以处理页面上发生的某种
    特定类型的事件。这样做与采取传统的做法相比具有如下优点。

    - document 对象很快就可以访问，而且可以在页面生命周期的任何时点上为它添加事件处理程序

      （无需等待DOMContentLoaded 或load 事件）。换句话说，只要可单击的元素呈现在页面上，

      就可以立即具备适当的功能。

    -  在页面中设置事件处理程序所需的时间更少。只添加一个事件处理程序所需的DOM 引用更少，

      所花的时间也更少。

    - 整个页面占用的内存空间更少，能够提升整体性能。

      最适合采用事件委托技术的事件包括click、mousedown、mouseup、keydown、keyup 和keypress。

      虽然mouseover 和mouseout 事件也冒泡，但要适当处理它们并不容易，而且经常需要计算元素的位置。（因为当鼠标从一个元素移到其子节点时，或者当鼠标移出该元素时，都会触发mouseout 事件。）



​	**移除事件处理程序：**

​	每当将事件处理程序指定给元素时，运行中的浏览器代码与支持页面交互的JavaScript 代码之间就
​	会建立一个连接。这种连接越多，页面执行起来就越慢。如前所述，可以采用事件委托技术，限制建立
​	的连接数量。另外，在不需要的时候移除事件处理程序，也是解决这个问题的一种方案。内存中留有那
​	些过时不用的“空事件处理程序”（dangling event handler），也是造成Web 应用程序内存与性能问题的
​	主要原因。
​	在两种情况下，可能会造成上述问题。第一种情况就是从文档中移除带有事件处理程序的元素时。
​	这可能是通过纯粹的DOM操作，例如使用removeChild()和replaceChild()方法，但更多地是发
​	生在使用innerHTML 替换页面中某一部分的时候。如果带有事件处理程序的元素被innerHTML 删除
​	了，那么原来添加到元素中的事件处理程序极有可能无法被当作垃圾回收。来看下面的例子。

```html
<div id="myDiv">
	<input type="button" value="Click Me" id="myBtn">
</div>
<script type="text/javascript">
	var btn = document.getElementById("myBtn");
	btn.onclick = function(){
		//先执行某些操作
		document.getElementById("myDiv").innerHTML = "Processing..."; //麻烦了！
	};
</script>
```

​	这里，有一个按钮被包含在<div>元素中。为避免双击，单击这个按钮时就将按钮移除并替换成一
​	条消息；这是网站设计中非常流行的一种做法。但问题在于，当按钮被从页面中移除时，它还带着一个
​	事件处理程序呢。在<div>元素上设置innerHTML 可以把按钮移走，但事件处理程序仍然与按钮保持
​	着引用关系。有的浏览器（尤其是IE）在这种情况下不会作出恰当地处理，它们很有可能会将对元素和
​	对事件处理程序的引用都保存在内存中。如果你知道某个元素即将被移除，那么最好手工移除事件处理
​	程序，如下面的例子所示。

```html
<div id="myDiv">
	<input type="button" value="Click Me" id="myBtn">
</div>
<script typ	e="text/javascript">
	var btn = document.getElementById("myBtn");
	btn.onclick = function(){
		//先执行某些操作
		btn.onclick = null; //移除事件处理程序
		document.getElementById("myDiv").innerHTML = "Processing...";
	};
</script>
```

​	在此，我们在设置<div>的innerHTML 属性之前，先移除了按钮的事件处理程序。这样就确保了内存可以被再次利用，而从DOM 中移除按钮也做到了干净利索。
注意，在事件处理程序中删除按钮也能阻止事件冒泡。目标元素在文档中是事件冒泡的前提。

​	采用事件委托也有助于解决这个问题。如果事先知道将来有可能使用innerHTML替换掉页面中的某一部分，那么就可以不直接把事件处理程序添加到该部分的元素中。而通过把事件处理程序指定给较高层次的元素，同样能够处理该区域中的事件。

​	导致“空事件处理程序”的另一种情况，就是卸载页面的时候。毫不奇怪，IE8 及更早版本在这种情况下依然是问题最多的浏览器，尽管其他浏览器或多或少也有类似的问题。如果在页面被卸载之前没有清理干净事件处理程序，那它们就会滞留在内存中。每次加载完页面再卸载页面时（可能是在两个页面间来回切换，也可以是单击了“刷新”按钮），内存中滞留的对象数目就会增加，因为事件处理程序占用的内存并没有被释放。
​	一般来说，最好的做法是在页面卸载之前，先通过onunload 事件处理程序移除所有事件处理程序。在此，事件委托技术再次表现出它的优势——需要跟踪的事件处理程序越少，移除它们就越容易。对这种类似撤销的操作，我们可以把它想象成：只要是通过onload 事件处理程序添加的东西，最后都要通过onunload 事件处理程序将它们移除。

25. ​



25. ​

    ​

26. **作用域安全的构造函数**

    《JavaScript高级程序设计》（第3版）第22章里边第一些高级技巧还是很有用的，在之前去公司面试的时候都有问到。

    ​

27. **函数绑定**

    在头条面试的时候问到了这部分的内容，尤其是关于bind()的。

    ​

28. **函数柯里化**