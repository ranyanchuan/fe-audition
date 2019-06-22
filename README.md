#### 前端面试
***
##### 1、浏览内核有哪些？
1. ie-->trident
2. firefix-->gecko
3. safari-->webkit
4.chrome-->blink
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

