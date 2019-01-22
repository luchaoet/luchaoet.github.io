---
title: es5继承
date: 2018-11-02 17:29:51
tags: js
summary: es5继承方法总结
---
### 原型对象/构造函数/实例对象


![1536826651758-ea6fcc46-cf1b-4a41-9321-1809f5ee0649 下午4.34.22.png | center | 677x283](https://cdn.nlark.com/yuque/0/2018/png/115449/1541062307311-9ada2a77-ffe3-4121-a6d3-10fb3344dc3a.png "")

每创建一个函数，该函数的`prototype`属性都会指向一个对象，这个对象就是原型对象
原型对象默认有个`constructor`属性，指向构造函数
构造函数实例出来的对象有一个`__proto__`属性，指向原型对象
所以实例能够访问原型对象上的所有属性和方法

__所以三者的关系是，每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。__<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">通俗点说就是，实例通过内部指针可以访问到原型对象，原型对象通过constructor指针，又可以找到构造函数。</span></span>

实例化
```javascript
// 构造函数
function Person(name){
	this.name = name;
}
Person.prototype.eat = function() {
    console.log(this.name + '正在吃饭')
}

// 实例化一个lucy对象
const lucy = new Person('Lucy');
lucy.eat(); // Lucy正在吃饭
```
以上代码定义了一个构造函数`Person()`
实例lucy由于其内部`__proto__`指向了原型对象，所以可以访问到eat方法


![微信图片_20181208225138.png | center | 747x306](https://cdn.nlark.com/yuque/0/2018/png/115449/1544280883202-83729260-bfbf-4779-b0aa-e63cb9b55291.png "")

<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">prototype 只是一个指针，指向的是原型对象</span></span>
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">但是这个原型对象并不特别，它也只是一个普通对象</span></span>
修改<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">prototype，使之比在指向最初的实例</span></span>
```javascript
// 构造函数
function Person(name){
	this.name = name;
}
const lucy = new Person('Lucy');

function Dog(name){
	this.name = name;
}
// 修改Dog.protptype，指向Person的实例
Dog.prototype = lucy;

const dog = new Dog('旺财');
console.log(dog.__proto__)
```


![屏幕快照 2018-11-01 下午6.58.13.png | center | 484x108](https://cdn.nlark.com/yuque/0/2018/png/115449/1541069928654-fad4362d-70e2-4aec-888d-cb485b732db2.png "")


### 1. 原型链继承
所有的实例的`__proto__`属性，指向原型对象，并且可以访问原型对象上的所有属性和方法
如果a的原型对象`prototype`另一个类的实例b，这个实例又会指向一个新的原型对象B，那么a就能访问b的实例属性和原型B的所有属性和方法
同理，新的原型对象B又指向另外一个对象的实例c，这个实例又会指向新的原型对象C，那么a又能访问c的实例属性和原型对象C的所有属性和方法
这就是js通过原型链继承的方法
```javascript
//定义一个 Animal 构造函数，作为 Dog 的父类
function Animal () {
    this.superType = 'Animal';
}
Animal.prototype.superSpeak = function () {
    console.log(this.superType);
}

function Dog (name) {
    this.name = name;
    this.type = 'Dog';  
}
//改变Dog的prototype指针，指向一个 Animal 实例
Dog.prototype = new Animal();
//上面那行就相当于这么写
//var animal = new Animal();
//Dog.prototype = animal;

Dog.prototype.speak = function () {
	console.log(this.type);
}
var doggie = new Dog('jiwawa');
doggie.superSpeak();  //Animal
console.log(doggie instanceof Animal); //true 
console.log(doggie instanceof Dog); //true
```
首先定义一个构造函数Animal，<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">通过new Animal()得到实例，会包含一个实例属性 superType 和一个原型属性 superSpeak</span></span>
后面，Animal实例覆盖掉Dog的原型对象，<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">当 doggie 去访问superSpeak属性时，js会先在doggie的实例属性中查找，发现找不到，然后，js就会去doggie 的原型对象上去找，doggie的原型对象已经被我们改成了一个animal实例，那就是去animal实例上去找。先找animal的实例属性，发现还是没有 superSpeack, 最后去 animal 的原型对象上去找，这才找到了</span></span>
于是，我们便通过原型链的方式，实现了Dog继承Animal的所有属性和方法
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">总结来说：就是当重写了Dog.prototype指向的原型对象后，实例的内部指针也发生了改变，指向了新的原型对象，然后就能实现类与类之间的继承了</span></span>

特点：
* 实例是子类的实例，也是父类的实例
* 父类新增原型方法和属性，子类都能使用

缺点：
* 因为`prototype`被重新赋值`Dog.prototype = new Animal()`，后面想为子类新增原型属性和方法，必须要在`new Animal()`这样的语句之后执行，不能放到构造器中
* 不能像构造继承一样实现多继承
* 来自原型对象的所有属性和方法被所有实例共享
* 创建子类实例时，不能向父类构造函数传参

### 2. 构造继承
使用父类的构造函数来增强子类实例，等于是复制父类的实例给子类（没用到原型）
```javascript
function Person(name){
	this.name = name;
	this.sleep = function() {
		console.log('人以睡为天')
	}
}
Person.prototype.eat = function() {
    console.log(this.name + '正在吃饭')
}

function Chinese(name){
    // 扩展构造函数
	Person.call(this);
	this.name = name;
}

var lucy = new Chinese('lucy');
console.log(lucy.name); // lucy
lucy.sleep(); // 人以睡为天
// lucy.eat(); // 报错 说明不能继承原型属性和方法
/**
 * lucy不是Person的实例 
 * 而是Chinese的实例
 */
console.log(lucy instanceof Person); // false 
console.log(lucy instanceof Chinese); // true
```

特点：
* 创建子类实例时，可以向父类传递参数
* 可以多继承（call多个父类对象）

缺点：
* 实例是子类的实例，不是父类的实例
* 只能继承父类的实例属性和方法，不能继承原型的属性和方法
* 不能实现函数复用，每个子类都有父类实例函数的副本，影响性能

### 3. 实例继承
<span data-type="color" style="color:rgb(0, 0, 0)"><span data-type="background" style="background-color:rgb(255, 255, 255)">为父类实例添加新特性，作为子类实例返回</span></span>
```javascript
function Person(name){
	this.name = name;
	this.sleep = function() {
		console.log('人以睡为天')
	}
}
Person.prototype.eat = function() {
    console.log(this.name + '正在吃饭')
}

function Chinese(name){
	var instance = new Person();
    // 可以传参
	instance.name = name;
	return instance;
}

var lucy = new Chinese('lucy');
console.log(lucy.name); // lucy
lucy.sleep(); // 人以睡为天
lucy.eat(); // lucy正在吃饭
console.log(lucy instanceof Person); // true 
console.log(lucy instanceof Chinese); // false
```

特点：
* 不限制调用方式，`new Chinese('lucy')`和`Chinese('lucy')`效果是一样的

缺点：
* 实例是父类的实例，不是子类的实例
* 不支持多继承

### 4. 拷贝继承
```javascript
function Person(name){
	this.name = name;
	this.sleep = function() {
		console.log('人以睡为天')
	}
}
Person.prototype.eat = function() {
    console.log(this.name + '正在吃饭')
}

function Chinese(name){
	var person = new Person();
	for(var p in person){
		Chinese.prototype[p] = person[p];
	}
	Chinese.prototype.name = name;
}

var lucy = new Chinese('lucy');
console.log(lucy.name); // lucy
lucy.sleep(); // 人以睡为天
lucy.eat(); // lucy正在吃饭
console.log(lucy instanceof Person); // false 
console.log(lucy instanceof Chinese); // true
```

特点：
* 支持多继承

缺点：
* 效率低，内存占用高
* 无法获取父类不可枚举的方法

### 5. 组合继承
通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
```javascript
function Person(name){
	this.name = name;
	this.sleep = function() {
		console.log('人以睡为天')
	}
}
Person.prototype.eat = function() {
    console.log(this.name + '正在吃饭')
}

function Chinese(name){
	Person.call(this);
	this.name = name;
}
Chinese.prototype = new Person();

var lucy = new Chinese('lucy');
console.log(lucy.name); // lucy
lucy.sleep(); // 人以睡为天
lucy.eat(); // lucy正在吃饭
console.log(lucy instanceof Person); // true 
console.log(lucy instanceof Chinese); // true
```

特点：
* 可以继承实例的属性和方法，也可以继承原型属性和方法
* 既是子类的实例，也是父类的实例
* 可传参
* 函数可复用

缺点：
* 调用了两次父类构造函数

### 6. 寄生组合继承
```javascript
function Person(name){
	this.name = name;
	this.sleep = function() {
		console.log('人以睡为天')
	}
}
Person.prototype.eat = function() {
    console.log(this.name + '正在吃饭')
}

function Chinese(name){
	Person.call(this);
	this.name = name;
}
(function(){
	// 创建一个没有实例方法的类
	var Super = function(){};
	Super.prototype = Person.prototype;
	//将实例作为子类的原型
	Chinese.prototype = new Super();
})();
Chinese.prototype.constructor = Chinese;

var lucy = new Chinese('lucy');
console.log(lucy)
console.log(lucy.name); // lucy
lucy.eat(); // lucy正在吃饭
lucy.sleep(); // 人以睡为天
console.log(lucy instanceof Person); // true
console.log(lucy instanceof Chinese); //true
```

特点：
* 堪称完美

缺点：
* 实现太复杂

总结：
ES5的继承可以用下图来概括

![1290444-20180522190257058-425523963.png | center | 525x529](https://cdn.nlark.com/yuque/0/2018/png/115449/1541150060325-5d6ccac1-da3e-490d-9bd5-74c605f37502.png "")