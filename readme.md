## Javascript-Design-Pattern

> Javascript设计模式

---

- **第1章 灵活的语言——JavaScript**
- **第2章 写的都是看到的——面向对象编程**
- **第3章 神奇的魔术师——简单工厂模式**
- **第4章 给我一张名片——工厂方法模式**
- **第5章 出现的都是幻觉——抽象**
- **第6章 分即是合——建造者模式**
- **第7章 语言之魂——原型模式**
- **第8章 一个人的寂寞——单例模式**
- **第9章 套餐服务——外观模式**
- **第10章 水管弯弯——适配器模式**
- **第11章 牛郎织女——代理模式**
- **第12章 房子装修——装饰者模式**
- **第13章 城市间的公路——桥接模式**
- **第14章 超值午餐——组合模式**
- **第15章 城市公交车——享元模式** 
- **第16章 照猫画虎——模板方法模式**
- **第17章 通信卫星——观察者模式**
- **第18章 超级玛丽——状态模式**
- **第19章 活诸葛——策略模式**
- **第20章 有序车站——职责链模式** 
- **第21章 命令模式**
- **第22章 驻华大使——访问者模式**
- **第23章 媒婆——中介者模式**
- **第24章 做好笔录——备忘录模式**
- **第25章 点钞机——迭代器模式**
- **第26章 语言翻译——解释器模式**

---

### 第1章：灵活的语言——JavaScript

1.函数也是全局变量，当一个检测类多个函数时通常不用函数形式的依次创建，使用对象收编变量，同时可以实例化（拥有自己的一套方法），以下是类的创建方式：

```javascript
//类的创建方式
var object = function(){
    this.checkName = function(){

    }
    this.checkEmail = function(){

    }
    this.chekPassword = function(){

    }
}
```
- 2.绑定在对象类的`原型`上可以减少消耗:

```javascript
var CheckObject = function(){};
CheckObject.prototype = {
    checkName:function(){
        
    },
    checkEmail:function(){

    },
    chekPassword:function(){
        
    }
}
```
- 3.函数的祖先,javascrip一切函数皆对象。

```javascript
Function.prototype.checkEmail = function(){

}
//函数的方式调用
var f = funciton(){};
f.checkEmail();
//类的方式调用
var c = new Function();
c.checkEmail();
```

**不建议这样做，这样做会污染原生对象Function，可以抽象出一个统一添加方法的功能方法**

实现不污染原生Function的同时，链式添加和链式调用：

```javascript
//使用函数的方式进行调用
Function.prototype.addMethod = funciton(name, fn){
    this[name] = fn;
    return this;
}
var methods = function(){

}
methods.addMethod('checkName', funciton(){
    //验证姓名
   return this;//链式调用
}).addMethod('checkEmail', function(){
    //验证邮箱
    return this;
})

//使用类的方式进行调用
Function.prototype.addMethod = funciton(name, fn){
    this.prototype[name] = fn;
    return this;
}
//同时要使用new的方式进行创建新对象

```

---

### 第2章 写的都是看到的——面向对象编程

- 1.函数级作用域实现私有属性；
- 2.`this`创建公有方法，同时可以建立外部访问私有的特权方法；
- 3.通过外部点语法创建的公有属性和方法是外部不能访问的(不会添加到新创建的对象上)；
- 4.通过外部`prototype`创建的方法和属性是可以访问的；

```javascript
var Book = function(id, name, price) {
    //私有属性
    var num = 1;
    //私有方法
    function checkId() {};

    //特权方法
    this.getName = function() {};
    this.setName = function() {};

    //对象共有属性
    this.id = id;
    //对象公有方法
    this.copy = function() {};

    //构造器
    this.setName(name);
}
Book.isChinese = true;
Book.restTime = function() {
    console.log("new time");
};
Book.prototype = {
    //公有属性
    isJSBook: false,
    //公有方法
    display: function() {},
}

var b = new Book(11, "javascript", 50);
console.log(b.num); //->undefined
console.log(b.isJSBook); //->false
console.log(b.id); //->11
console.log(b.isChinese); //->undefined 
```
- 5.防止不写`new`关键词，设置检察长。

**漏写`new`实例化的后果**
```javascript
/*
实例化对象的时候没有写new关键词，就会直接执行这个函数，这个函数是全局的，所以添加到了全局属性当中去，此时全局是window，所以添加到了window上去。
*/
var Book = function(title, time, type){
    this.title = title;
    this.time = time;
    this.type = type;
}

var book = Book('java', '2014', 'js');
console.log(book);//undefined
console.log(window.title)//java
console.log(window.time);//2014
console.log(window.type);//js
```
**设置检察长来防止漏写关键词`new`**

> 类包含三部分：1.构造函数内的，供实例化对象复制用；2.构造函数外，通过点语法创建的；3.原型中的，通过原型链间接访问。

```javascript

var Book = function(title, time, type){
    //判断执行贵哦称重this是否是当前这个对象
    if(this instanceof Book){
        this.title = title;
        this.time = time;
        this.type = type;
    }else{
        //否则创建这个对象
        return new Book(title, time, type);
    }

}

var book = Book('java', '2014', 'js');
console.log(book);//Book
console.log(book.title)//java
console.log(book.time);//2014
console.log(book.type);//js
console.log(window.title)//undefined
console.log(window.time);//undefined
console.log(window.type);//undefined
```
- 6.继承：子类原型对象类式继承。

```javascript
/*类式继承*/
//声明父类
function SuperClass() {
    this.superValue = true;
}
//为父类添加公有方法
SuperClass.prototype.getSuperValue = function() {
        return this.superValue;
    }
    //声明子类
function SubClass() {
    this.SubValue = false;
}
//继承父类
SubClass.prototype = new SuperClass();
//为子类添加公有方法
SubClass.prototype.getSubValue = function() {
    return this.SubValue;
}

var instance = new SubClass();
console.log(instance.getSuperValue()); //true
console.log(instance.getSubValue());//false
console.log(instance instanceof Object);//true 实例是原生对象Object的实例
```
**缺点：子类的一个实例更改了子类原型从父类构造函数中继承来的共有属性，将会直接影响其他子类；优点：继承了原型方法**

```javascript
function SuperClass() {
    this.books = ['Javascript'];
}

function SubClass() {}

SubClass.prototype = new SuperClass();
var instance1 = new SubClass();
var instance2 = new SubClass();

console.log(instance2.books);
instance1.books.push('设计模式');//instance1修改继承来的属性
console.log(instance2.books);//instance2得到了改变
```

- 7.继承：构造函数继承

**缺点：没有涉及原型，没法继承父类原型方法；优点：继承的共有属性，单独一份**

```javascript
/*构造函数式继承*/
//声明父类
function SuperClass(id) {
    //引用类型的共有属性
    this.books = ['Javascript'];
    //值类型共有属性
    this.id = id;
}
//父类声明原型方法
SuperClass.prototype.showBooks = function() {
    console.log(this.books);
}

function SubClass(id) {
    SuperClass.call(this, id);
}
//创建一个子类的实例
var instance1 = new SubClass(10);
var instance2 = new SubClass(11);

instance1.books.push("设计模式");
console.log(instance1.books); //[ 'Javascript', '设计模式' ]
console.log(instance1.id); //10
console.log(instance2.books); //[ 'Javascript' ]
console.log(instance2.id); //11

instance1.showBooks(); //TypeError 父类原型方法不能被继承
```
- 8.继承：组合继承

**结合了类式继承和构造函数继承的优点**
继承了共有的父类原型方法，同时还有独自的属性和方法，完美继承
```javascript
/*组合式式继承*/
//声明父类
function SuperClass(name) {
    //引用类型的共有属性
    this.books = ['Javascript'];
    //值类型共有属性
    this.name = name;
}
//父类声明原型共有方法
SuperClass.prototype.getName = function() {
        console.log(this.name);
    }
    //声明子类
function SubClass(name, time) {
    //构造函数式继承父类name属性
    SuperClass.call(this, name);
    //子类新增共有属性
    this.time = time;
}
//类式继承 子类原型继承父类
SubClass.prototype = new SuperClass();
//子类型原型方法
SubClass.prototype.getTime = function() {
    console.log(this.time);
}

var instance1 = new SubClass("js", 2012);
instance1.books.push("java");
console.log(instance1.books);//[ 'Javascript', 'java' ]
instance1.getName();//js
instance1.getTime();//2012

var instance2 = new SubClass("css", 2013);
instance2.books.push("python");
console.log(instance2.books);//[ 'Javascript', 'python' ]
instance2.getName();//css
instance2.getTime();//2013
```

- 9. 











