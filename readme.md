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







