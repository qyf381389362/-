### ES6笔记

---

#### 1.let和const

let 定义变量

const定义常量

#### 2.解构赋值

数组解构赋值：

```javascript
let[a,b,c,d] = ["aa","bb",77,88];
```

嵌套数组解构：

```javascript
let [a,b,[c,d],e] =["aa",'bb',[33,44],55]; 
```

空缺变量：

```javascript
let [a,b,,e] =["aa",'bb',[33,44],55]; 
alert(e);//55
```

多余变量：

```javascript
let [a,b,,e,f] =["aa",'bb',[33,44],55]; 
alert(f);//undefined
```

默认值:

```javascript
let [a,b,,e,f='hello'] =["aa",'bb',[33,44],55]; 
alert(f);//hello
```

对象解构1：

```javascript
 let obj = new Object();
 obj.uid = 111;
 obj.uname = '张三';
 let {uid:id,uname:name} = obj;
 alert(name);//张三
```

对象解构2：

```javascript
 let obj = new Object();
 obj.uid = 111;
 obj.uname = '张三';
 let uid,uname;
({uid,uname}=obj);//注意这里要用括号括起来
 alert(uname);//张三
```

嵌套：

```javascript
let obj = new Object();
obj.uid = 111;
obj.uname = '张三';
obj.arr = ['aa','bb'];
let uid,uname,arr,a,b;
({arr:[a,b]}=obj);//注意这里要用括号括起来
alert(a);//aa
```

字符串解构：

```javascript
let [a,b,c,d] = "倚天屠龙";
alert(c);//屠
```

函数参数解构：

```javascript
function analysis({uid,uname}){ 
    alert(uid); 
    alert(uname); 
} 
analysis(obj); 
```

#### 3.Symbol(值类型数据，唯一的)

Symbol是ES6新增的一种值类型数据，表示一种绝不重复的值

```javascript
let s1 = Symbol(33); 
let s2 = Symbol(33); 
alert(s1.toString()); //Symbol(33)
alert(s1==s2);       //false
```

#### 4.Set和WeakSet数据结构

Set和WeakSet 数据结构是ES6新增。   
Set它与数组非常相似，但是Set数据结构的成员都是唯一的。特别说明:Set中只能添加一个NaN。 

```javascript
var set = new Set([1, 2, 3, 4, 2, 8, 4]); //两个2,4   
for (var elem of set) {//for of 遍历   
  console.log(elem);  //1 2 3 4 8
}   
```

另外一种声明方式：

```javascript
var set = new Set(); 
for(let i = 0; i<10;i++){
  set.add(i);
}
for (var elem of set) {//for of 遍历   
  console.log(elem);  //0 1 2 3 4 5 6 7 8 9
}   
```

```javascript
var set = new Set(); 
[1,2,3,4,2,8,4].map(function(elem){
  set.add(elem);
});
for (var elem of set) {//for of 遍历   
  console.log(elem);  //1 2 3 4 8
}   
```

WeakSet它与Set十分相似，对象的值也不能是重复的，与Set不同点:   
1.WeakSet成员只能够是对象。   
2.作为WeakSet成员的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中。这个特点意味着，无法引用WeakSet的成员，因此WeakSet是不可遍历的。   
3.使用WeakSet存储对象实例的好处是，由于是对对象实例的引用，不会被计入内存回收机制，所以删除实例的时候，不用考虑Weakset，也不会出现内存泄漏。

 weakset不能取值，也不能显示，只用来表示是否有重复的对象 。比如判断是否有对象bObj: weakset.has(bObj)      

#### 5.Map和WeakMap数据结构

  Map

它们本质与对象一样，都是键值对的集合，但是他们与Object对象主要的不同是，键可以是各种类型的数值，而Object对象的键只能是字符串类型或者Symbol类型值。Map和WeakMap是更为完善的Hash结构

WeakMap数据结构 
WeakMap结构与Map结构基本类似。 
区别是它只接受对象作为键名，不接受其他类型的值作为键名。键名是对象的弱引用，当对象被回收后，WeakMap自动移除对应的键值对，WeakMap结构有助于防止内存泄漏。 

由于WeakMap对象不可遍历，所以没有size属性。 

#### 6.Iterator遍历器

遍历器是一种接口，它为不同的数据结构提供了统一的访问机制。 
如果一个数据结构具有遍历器接口，那么就可以依次处理该数据结构的成员。 
当前javascript用来表示集合的数据结构有四种，分别是数组、对象、Set和Map，并且这四种数据结构可以相互嵌套使用，比如数组的成员可以是对象，对象的成员又可以是Set等等。 

#### 7.Generator函数

Generator函数是ES6新增的一种异步编程方案。 
说明:Generator函数指的是一种新的语法结构,是一个遍历器对象生成器,它内部可以封装多个状态，非常适合用于异步操作。 

Generator函数语法和普通的function函数类似，但是有三个不同点: 
（1）function关键字和函数名称之间有一个星号（*）。 
（2）函数体内可以使用yield语句。 
（3）函数调用后不会立即执行，返回的是一个遍历器对象。 

yield语句: 
每一个yield语句定义不同的状态,它也是一个代码执行暂停标识。 
yield语句不能在普通函数中使用，否则会报错。 
调用Generator函数可以返回一个遍历器对象,要想访问Generator函数中的每一个状态，需要使用遍历器对象调用next()方法。 

如果yield语句作为其他语句的一部分，那么必须使用小括号包裹，否则会报错 
function *yuanku() { 
  //console.log("欢迎来到" + yield "源库网");//报错 
  console.log("欢迎来到" + (yield "源库网"));//正确 
} 

面试的时候可能问到的题类型：

向next中传值，!!!此值作为上一个yield的返回值!!! 

```javascript
function* yuanku(num) { 
  let x = 2 * (yield num); 
  console.log('x='+x); 
  let y = yield x*3; 
  console.log('y='+y); 
  console.log(x,y); 
} 
var g=yuanku(5); 
console.log(g.next());
console.log(g.next()); 
console.log(g.next(3));
console.log(g.next(3));
```
#### 8.Promise对象

传统实现异步操作就是采用回调函数，回调函数方式本身没有什么问题，但是在多重回调层层嵌套的情况下，那么代码的可阅读性就会出现问题。
Promise对象是一个新的异步操作解决方案，比原有的回调函数等方式更为合理
Promise对象具有三种状态：Pending（等待）、Resolved（已完成）和Rejected（不能完成，出错了）。
Promise对象状态的改变只有两种可能：Pending转变为Resolved或者Pending转变为Rejected。

```javascript
step1().then(step2).then(step3).then(step4).catch(function(err){
  // do something when err
});
```
#### 9.箭头函数

如果箭头表达式仅仅就是简化了函数的命名，我们为什么要改变原来的习惯而去使用它呢？
箭头函数内部没有constructor方法，也没有prototype，所以不支持new操作。但是它对this的处理与一般的普通函数不一样。箭头函数的 this 始终指向函数定义时的this，而非执行时。

```javascript
var obj ={
     x:121,
     func:function(){
       console.log(this.x);
     },
     test:function(){
       setTimeout(function(){
         alert(this);
         this.func();//this指针转为全局
       },1000);
     }
   };
   obj.test();//alert [object Window]  func is undefined
```

```javascript
var obj ={
    x:121,
    func:function(){
      console.log(this.x);
    },
    test:function(){
      setTimeout(()=>{
        alert(this);//始终指向obj,使用起来比较安全
        this.func()；
      },1000);
    }
  };
  obj.test();//alert [object Object] //121
```

#### 10.JS面向对象

面向对象的三个特点：封装、继承、多态。

JS有封装和继承，没有多态，因为JS是一种弱类型语言。