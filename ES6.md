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