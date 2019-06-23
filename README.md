#### 前端面试
[前端工匠公众号github](https://github.com/ljianshu/Blog)

***
##### 1、浏览内核有哪些？
1. ie-->trident
2. firefix-->gecko
3. safari-->webkit  
4. chrome-->blink
##### 2、浏览器是如何渲染页面的？
1.解析生成dom tree
2.解析生成 css rule tree
3.将 dom tree与css rule tree 合成 render tree
4.layout 计算每一个节点的位置
5.painting 开始渲染
6.reflow 当位置发生变化，回流
7.repaint 重绘，只改变内部样式 backgroun-color

##### 3、何优化首屏渲染性能？

##### 4、jsonp 工作原理？

##### 5、同源策略的优点和缺点？

##### 6、ajax请求原理？

##### 7、[javascript 面向对象中继承的实现？](https://www.cnblogs.com/chaixiaozhi/p/8515087.html)
1.原型链继承:将父类的示例作为子类的原型 Parent.prototype=new Person();
2.构造继承: 利用call或者apply 把父类中的this指定的属性和方法复制到子类来 apply()、call(),不能复制父类的原型属性和方法;
```js
 function Parent = (age) {
          Person.call(this,'老明');　　//这一句是核心关键
          //这样就会在新parent对象上执行Person构造函数中定义的所有对象初始化代码，
          // 结果parent的每个实例都会具有自己的friends属性的副本
          this.age = age;
  };
```
3.组合继承：通过调用父类构造，继承父类的属性并保留传参的优点，然后再通过将父类实例作为子类原型，实现函数复用
```js
function Person  (name) {
     this.name = name;
     this.friends = ['小李','小红'];
 };

Person.prototype.getName = function () {
     return this.name;
 };

function Parent (age) {
    Person.call(this,'老明');　　//这一步很关键
    this.age = age;
};

Parent.prototype = new Person('老明');　　//这一步也很关键
var result = new Parent(24);
```
4.寄生组合继承:通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
```js
function Person(name) {
    this.name = name;
    this.friends = ['小李','小红'];
}

Person.prototype.getName = function () {
    return this.name;
};

function Parent(age) {
    Person.call(this,"老明");
    this.age = age;
}

(function () {
    var Super = function () {};     // 创建一个没有实例方法的类
    Super.prototype = Person.prototype;
    Parent.prototype = new Super();     //将实例作为子类的原型
})();

var result = new Parent(23);
console.log(result.name);
console.log(result.friends);
console.log(result.getName());
console.log(result.age);
```
##### [8、js事件冒泡和js事件代理？](https://www.cnblogs.com/sspeng/p/9719854.html)
1.事件冒泡: 当子元素的事件处理函数被触发（如onclick），该事件会从事件源（当前子元素）逐级向上层元素传递，触发祖先元素的 onclik 事件，一直到最外层 html 根元素。这可能会带来困扰，不必要的事件处理函数被执行了，不过我们可以阻止事件冒泡。事件触发时，会传入一个event对象，它有一个 stopPropagation() 方法可以阻止事件冒泡。 事件冒泡机制当然也有有利的一面，事件代理就是基于浏览器的事件冒泡机制。  
2.事件代理: 事件代理也叫事件委托，当我们需要为父元素的很多子元素添加事件时，可以通过把事件添加到父元素并把事件委托给父元素来触发事件处理函数。 在开发中，我们有时会遇到给列表每一个子元素都添加一个事件，可以用遍历来操作，这种方法固然简单，但是如果这个列表有巨量的子元素的时候，就要消耗大量的性能，并且当子元素需要新增的时候，每增加一个子元素就需要遍历一次，这种方法就更不可取。 事件委托不仅实现相同了功能，而且大大减少了DOM操作。
```js
<ul class="wrap">
    <li class="item">1111<button>删除</button></li>
    <li class="item">2222<button>删除</button></li>
    <li class="item">3333<button>删除</button></li>
    <li class="item">4444<button>删除</button></li>
    <li class="item">5555<button>删除</button></li>
</ul>
<button class="add">添加子元素</button>

<script>
    let oWrap = document.getElementsByClassName('wrap')[0];
    let oItem = document.getElementsByClassName('item');
    let oAdd = document.getElementsByClassName('add')[0];

    oWrap.addEventListener('click',function(e){
        //判断事件目标元素是否为 li ,并显示它的第一个子节点的文本内容
        if(e.target && e.target.nodeName.toLowerCase() == 'li'){
            console.log(e.target.childNodes[0].textContent);
        }

        //判断事件目标元素是否为 button ,删除它的父元素
        if(e.target && e.target.nodeName.toLowerCase() == 'button'){
            oWrap.removeChild(e.target.parentNode);
        }
    })

    //添加子节点
    oAdd.addEventListener('click',function () { 
        let oLi = document.createElement('li');
        oLi.setAttribute('class','item');
        oLi.innerHTML = oItem.length+1+'<button>删除</button>';
        oWrap.appendChild(oLi);
    })
</script>
```
##### [9、数组的排序方法](https://www.cnblogs.com/yuqing6/p/8862436.html)
```js
// 选择排序
function sort(arr) {
    for (let i = 0; i < arr.length - 1; i++) {
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[i] > arr[j]) {
                // es6 新语法
                // [arr[i], arr[j]] = [arr[j], arr[i]];
                let temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}

// console.log(sort([6, 5, 1, 0, 1, 2, 0, 3, 4, 1]))

// 冒泡排序
function bubbleSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // es6 新语法
                // [arr[i], arr[j]] = [arr[j], arr[i]];
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

// console.log(bubbleSort([6, 5, 1, 0, 1, 2, 0, 3, 4, 1]))
// 插入排序
function insertSort(arr) {
    var len = arr.length;
    var preIndex, current;
    for (let i = 1; i < len; i++) {
        preIndex = i - 1;
        current = arr[i];
        while (preIndex >= 0 && arr[preIndex] > current) {
            arr[preIndex + 1] = arr[preIndex];
            preIndex--;
        }
        arr[preIndex + 1] = current;
    }
    return arr;
}
```
##### 9、javascript 数据类型
1. null、undefined、boolean、string、number、symbol 和 object(引用类型)

##### 10、[数据类型的判断](https://mp.weixin.qq.com/s/WfEOY57Trq6JD1CO2VSYWA)

1、typeof 判断数据类型 
typeof返回一个表示数据类型的字符串，返回结果包括：number、boolean、string、symbol、object、undefined、function等7种数据类型，但不能判断null、array等
```js
typeof Symbol(); // symbol
typeof ''; // string
typeof true; // boolean
typeof undefinded; // undefinded
typeof new Function(); // function
typeof null; // object
typeof []; // object
typeof new Date(); // object
typeof new ReExp(); // object
typeof NaN; // number
```
2、instanceof
instanceof 是用来判断A是否为B的实例，表达式为：A instanceof B，如果A是B的实例，则返回true,否则返回false。instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性，但它不能检测null 和 undefined
```js
[] instanceof Array; // true
{} instanceof Objext; // true
new Date instanceof Date; // true
new RegExp() instanceof RegExp; // true
null instanceof Null; // 报错
undefined instanceof undefined; // 报错
NaN instanceof Nan; // 报错
```
3、constructor
constructor作用和instanceof非常相似。但constructor检测 Object与instanceof不一样，还可以处理基本数据类型的检测。不过函数的 constructor 是不稳定的，这个主要体现在把类的原型进行重写，在重写的过程中很有可能出现把之前的constructor给覆盖了，这样检测出来的结果就是不准确的。  
4、Object.prototype.toString.call();
Object.prototype.toString.call() 是最准确最常用的方式。
```js
Object.prototype.toString.call(''); // [object String] 
Object.prototype.toString.call(1); // [object Number] 
Object.prototype.toString.call(true); // [object Boolean] 
Object.prototype.toString.call(undefined); // [object Undefined] 
Object.prototype.toString.call(null); // [object Null] 
Object.prototype.toString.call(new Function()); // [object Function] 
Object.prototype.toString.call(new Date()); // [object Date] 
Object.prototype.toString.call([]); // [object Array] 
Object.prototype.toString.call(new RegExp()); // [object RegExp] 
Object.prototype.toString.call(new Error()); // [object Error] 
```
##### 11.浅拷贝与深拷贝
浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。
1.Object.assign()：需注意的是目标对象只有一层的时候，是深拷贝
2.Array.prototype.concat()
3.Array.prototype.slice()
深拷贝就是在拷贝数据的时候，将数据的所有引用结构都拷贝一份
1.热门的函数库lodash，也有提供_.cloneDeep用来做深拷贝
2.jquery 提供一个$.extend可以用来做深拷贝
3.JSON.parse(JSON.stringify())
4.手写递归方法
```js
// 定义检查数据类型的功能函数
function checkedType(target){
   return Object.prototype.toString.call(target).slice(8,-1);
}

// 实现深度clone ---对象/数组
function deep(target){

  let [result,targetType]=[null,checkedType[target]];
  if(targetType==='Object'){
    result={};
  }else if(targetType==='Array'){
    result=[];
  }else{
    ruturn target;
  }
  
  for(let i in target){
  let value=target[i];
  //判断目标结构里的每一值是否存在对象/数组
  if(checkedType[value] ==='Object' || checkedType[value] ==='Array'){
      //对象/数组里嵌套了对象/数组
      //继续遍历获取到value值
     result[i]=clone(value)
  }else{
    //获取到value值是基本的数据类型或者是函数。
    result[i]=value;
  }
 }
 return result;
}
```

##### 12、作用域和闭包
1. 执行上下文和执行栈
执行上下文就是当前JavaScript代码被解析和执行时所在环境的抽象概念，JavaScript 中运行任何代码都是执行上下文中运行  
执行上下文的生命周期包括三个阶段：`创建阶段-->执行阶段-->回收阶段`,我们会重点介绍创建阶段  
创建阶段(当函数被调用，但为执行任何其内部代码之前)，会执行以下三件事    
①、创建变量对象：首先初始化函数的参数arguments,提示函数声明和变量声明  
②、创建作用域链：
③、确定 this 指向：
```js
function test(arg){
 // 1. 形参arg 是 'hi'
 // 2. 因为函数声明比变量声明优先级高，所有 arg 是 function
 console.log(arg);
 var arg='hello'; // var arg 被忽略， arg='hello';
 function arg(){
   console.log("hello world");
 }
 console.log(arg());
}

test('hi');

```
JavaScript 引擎创建了执行栈来管理执行上下文。可以把执行栈认为是一个存储函数调用的栈结构，遵循先进后出的原则。
![avatar](https://mmbiz.qpic.cn/mmbiz_gif/zewrLkrYfsP7nDsOqPWQaGXND21JicTYEZSUPOIWAQN2bZZlx3hBG0uEWw19Er1MnDBDuLZlwwTYx5hxNSpkPrQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)
我们需要记住几个关键点： 

JavaScript执行在单线程上，所有的代码都是排队执行。   
一开始浏览器执行全局的代码时，首先创建全局的执行上下文，压入执行栈的顶部。  
每当进入一个函数的执行就会创建函数的执行上下文，并且把它压入执行栈的顶部。当前函数执行完成后，当前函数的执行上下文出栈，并等待垃圾回收。  
浏览器的JS执行引擎总是访问栈顶的执行上下文。  
全局上下文只有唯一的一个，它在浏览器关闭时出栈。 

2、作用域与作用域链  
① ES6 到来JavaScript 有全局作用域、函数作用域和块级作用域（ES6新增）。我们可以这样理解：作用域就是一个独立的地盘，让变量不会外泄、暴露出去。也就是说作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。  
```js
var a=100;
function fun(){
 var b=200; // 函数作用域
 console.log(a); // 全局作用域
 console.log(b);
}
fun();

```
② 自由变量的值如何得到 —— 向父级作用域(创建该函数的那个父级作用域)寻找。如果父级也没呢？再一层一层向上寻找，直到找到全局作用域还是没找到，就宣布放弃。这种一层一层的关系，就是作用域链 。
```js
function F1(){
 var a=100;
 return function(){
   console.log(a);
 }
}
function F2(f1){
 var a=200;
 console.log(f1())
}

var f1=F1();
F2(f1);

```

3. 闭包
闭包就是函数中的函数，里面的函数可以访问外面的函数的变量，外面的变量是内部函数的一部分
①、使用闭包可以访问函数中的变量
②、可以时变量长期保存到内存中，生命周期比较长
闭包不能滥用，否则会导致内存泄露，影响网页的性能。闭包使用完了后，要立即释放资源，将引用变量指向null  
主要应用场景
①、函数作为参数传递
②、函数作为返回值
```js
function outer(){
  var num=0;
  return function add(){
    num+++;
    console.log(num);
  }
}
var func1=outer();
func1(); // 实际上调用add(), 1
func1(); // 输出 2
fun1=null; // 释放资源

var func2=outer();
func2(); // 输出 1
func2(); // 输出 2
```
##### 异步
1、异步和同步  
```js
// 同步
console.log(100);
alert(200);
console.log(200);
// 100 200 300

// 异步
console.log(100);
setTimeout(function(){
  console.log(200);
},1000);
console.log(300);
// 100 300 200

```



