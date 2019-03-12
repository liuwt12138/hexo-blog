---
title: JavaScript中的六中继承方式
date: 2019-03-12 16:24:32
tags:
---

```js
//Javascript继承的几种方式
//参考：https://www.cnblogs.com/humin/p/4556820.html

//首先定义父类Animal
function Animal(name) {
    //name属性
    this.name = name || 'Animal';
    //实例方法
    this.sleep = function () {
        console.log(this.name + 'is sleeping');
    }
}
//原型方法
Animal.prototype.eat = function (food) {
    console.log(this.name + 'is eating' + food);
}

//前置内容：js中的new关键字做了什么
//——————————————————————————————————
//使用new关键字调用函数（new Animal(…)）的具体步骤：
//1. 创建空对象obj；
　　var obj = {};
//2. 设置新对象的constructor属性为构造函数的名称，将新对象的__proto__属性指向构造函数的prototype对象；
　　obj.__proto__ = Animal.prototype;
//3. 使用新对象调用构造函数，函数中的this被指向新实例对象：
　　Animal.call(obj);　　//{}.构造函数();          
//4. 将初始化完毕的新对象地址，保存到等号左边的变量中

//注意：若构造函数中返回this或返回值是基本类型（number、string、boolean、null、undefined）的值，则返回新实例对象；若返回值是引用类型的值，则实际返回值为这个引用类型。
//——————————————————————————————————

//以下为正文
//----------------------------------------------------------------
//一、原型链继承
// 将父类的实例作为子类的原型
function Dog() {

}
Dog.prototype = new Animal();
Dog.prototype.name = 'dog';
//test
var dog1 = new Dog();
console.log(dog instanceof Animal); //true
console.log(dog instanceof Dog); //true

//这种方法的特点：
//1、实例不仅是子类的实例，也是父类的实例
//2、父类中或者其原型链中新增的属性和方法，子类都可以访问
//3、缺点：如果想要为子类新增原型属性和方法，必须要放在``Dog.prototype = new Animal()``之后，不能放在构造函数中
//4、缺点：不能实现多继承
//5、缺点：创建子类实例使，无法向父类的构造函数传递参数
//6、缺点：来自原型对象的所有属性被所有实例共享，也就是说，当子类的一个实例改变了原型中的某个属性的值，子类的其它实例也会受其影响

//------------------------------------------------------------------
//二、构造继承
//使用父类的构造函数来增强子类的实例，相当于复制了父类的实例属性给子类
function Pig(name) {
    Animal.call(name);
    this.name = name || 'Pig';
}
//test
var pig = new Pig('edg_fans');
console.log(pig instanceof Animal); //false
console.log(pig instanceof Pig); //ture

//这种方法的特点：
//1、解决了方法一种，子类实例共享父类引用属性的问题
//2、创建子类的实例时，可以向父类传递参数
//3、可以通过call多个父类的构造函数的方式实现多继承
//4、缺点：实例不是父类的实例，仅是子类的实例
//5、缺点：只能继承父类的实例属性和方法，不能继承原型属性和方法，压根就没有用到原型
//6、缺点：无法实现函数复用，每个子类都有父类实例函数的副本，占用更多内存

//------------------------------------------------------------------
//三、实例继承
//为父类实例添加新特性，作为子类实例返回
function Cat(name) {
    var instance = new Animal(name);
    instance.name = name || 'Cat';
    return instance;
}
//test
var cat = new Cat('Tom');
console.log(cat instanceof Cat); //false
console.log(cat instanceof Animal); //true

//这种方法的特点：
//1、不限制调用方式，可以不使用new关键字，直接调用构造函数即可得到实例
//2、缺点：实例是父类的实例，不是子类的实例（和方法二相反）
//3、缺点：不支持多继承

//------------------------------------------------------------------------
//四、拷贝继承
function Mouse(name) {
    var animal = new Animal();
    for(var p in animal){
        Mouse.prototype[p] = animal[p];
    }
    Mouse.prototype.name = name || 'Jerry';
}
//test
var mouse = new Mouse();
console.log(mouse instanceof Animal); //false
console.log(mouse instanceof Mouse); //true

//这种方法的特点：
//1、支持多继承
//2、缺点：要拷贝父类的属性，效率低，消耗内存
//3、缺点：无法获取父类中[enumlable]为false的方法（不可枚举的方法，for in无法访问）

//--------------------------------------------------------------------------
//五、组合继承
//通过调用父类的构造函数，继承父类的属性实现传参，让后将父类实例作为子类的原型，实现函数复用
function Bull(name) {
    Animal.call(name);
    this.name = name || 'bull';
}
Bull.prototype = new Animal();
Bull.prototype.constructor = Bull;
//test
var bull = new Bull();
console.log(bull instanceof Animal); //true
console.log(bull instanceof Bull); //true

//这种方法的特点：
//1、弥补了方法二的缺点，既可以继承实例的属性和方法，也可以继承原型的属性和方法
//2、同时是子类和父类的实例
//3、可以想父类传递参数
//4、函数可以服用
//5、缺点：调用了两次父类的构造函数，生成了两份实例，只不过子类实例屏蔽了子类原型上的那一份

//--------------------------------------------------------------------------------
//六、寄生组合继承
//通过寄生方式，砍掉父类的实例属性。
//从而在调用两次父类的构造函数时，不会初始化两次实例的方法和属相，避免组合继承的缺点
function Frog(name){
    Animal.call(this);
    this.name = name || 'frog';
}
(function(){
    //创建一个没有实例方法的类
    var Super = function(){};
    Super.prototype = Animal.prototype;
    //将实例作为子类的原型
    Frog.prototype = new Super;
});
//修复constructor
Frog.prototype.constructor = Frog;
//test
var frog = new Frog('the_elder');
console.log(frog instanceof Frog); //true
console.log(frog instanceof Animal); //true

//特点：
//除了实现比较复杂，几乎完美

```