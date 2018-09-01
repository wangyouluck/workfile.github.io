vuejs or element ui use problem address

使用拓展运算符 ... 复制数组

// good
itemsCopy = [...items]

// better
function getFullName ({ firstName, lastName }) {
  return `${firstName} ${lastName}`
}


程序化生成字符串时，请使用模板字符串
const str = `ab${test}`

不要在非函数代码块中声明函数
// good
let test
if (isUse) {
  test = () => {
    // do something
  }
}

不要更改函数参数的值
function test (opts = {}) {
  // ...
}

使用标准的 ES6 模块语法 import 和 export
// better
import { Util } from './util'
export default Util

es6数组去重的两种方式

1.
let sq = ['aa','cc','bb','aa','33','bb'];
let news = new Set(sq);
console.log(news);
console.log([...news]);

Vue.filter('to-uppercase',function(value) {
	return value.toUpperCase();
})


Vue.filter('snippet',function(value) {
	return value.slice(0,100) + '...';
})

javascript:



---------------------------------------------------

function foo(num) {
	console.log('foo: '+ num);
	data.count++;
}
var data={
	count: 0;
}
var i;
for(i = 0;i <10;i++) {
	if(i>5) {
		foo(i);
	}
}
console.log(data.count);

22-----------------------------------------------------
function foo(num) {
	console.log('foo: '+ num);
	foo.count++;
}
foo.count = 0;

var i;
for(i = 0;i <10;i++) {
	if(i>5) {
		foo(i);
	}
}
console.log(foo.count);

33-------------------------------------------------
function foo(num) {
	console.log('foo: '+ num);
	this.count++;
}
foo.count = 0;

var i;
for(i = 0;i <10;i++) {
	if(i>5) {
		foo.call(foo, i);
	}
}
console.log(foo.count);

--------------------
function identify() {
	return this.name.toUpperCase();
}
function speak() {
	var greeting = "i'm "+ identify.call(this);
	console.log(greeting);
}
var me = {
	name: 'Kyle'
}
var you = {
	name: 'Reader'
}


speak.call(me);
speak.call(you);



identify.call(me);
identify.call(you);
===============================
function foo() {
	var a = 2;
	bar.call(bar, a);
}
function bar() {
	console.log(this.a)
}
foo();


function foo() {
	console.log(this.a)
}
var a = 2;
(function() {
	'use strict';
	foo();
})()
===============================

        .sidebar {
            background-color: #1b2133;
            // position: fixed;
            // z-index: 301;
            float: left;
			
		}
============================
function foo(something) {
	console.log(this.a, something);
	return this.a + something;
}
var obj = {
a: 2
}
var bar = function() {
	return foo.apply(obj, arguments);
}
var b = bar(3);
console.log(b);
===============================

function foo(something) {
	console.log(this.a, something);
	return this.a + something;
}
function bind(fn, obj) {
	return function() {
		return fn.apply(obj, arguments);
		//foo.apply(obj, 3)
	}
}

var obj = {
	a: 2
}
var bar = bind(foo, obj);//foo function + obj

var b = bar(3);
console.log(b);

============================
function foo(something) {
	console.log(this.a, something);
	return this.a + something;
}
var obj = {
a: 2
}
var bar = foo.bind(obj);

var b = bar(3);
console.log(b);

========================================================
function foo(el) {
	console.log(el, this.id);
}
var obj = {
	a : 2
}
[1,2,3].forEach(foo, obj);


========================================================


function foo() {
	console.log(this.a);
}
var obj1 = {
	a:2,
	foo: foo
}
var obj2 = {
	a:3,
	foo: foo
}
obj1.foo();
obj2.foo();

obj1.foo.call(obj2);
obj2.foo.call(obj1);
========================================================
function foo(something) {
	this.a = something;
}

var obj1 = {
	foo: foo
}
var obj2 = {}

obj1.foo(2);
console.log(obj1.a);

obj1.foo.call(obj2, 3);
console.log(obj2.a);

var bar = new obj1.foo(4);
console.log(obj1.a);
console.log(bar.a);


========================================================
function foo(something) {
	this.a = something;
}
var obj1 = {};
var bar = foo.bind(obj1);
bar(2);
console.log(obj1.a);

var baz = new bar(3);
console.log(obj1.a);
console.log(baz.a);


========================================================
function foo(p1, p2) {
	this.val = p1 + p2;
}
var bar = foo.bind(null, 'p1');
var baz= new bar('p2');
baz.val;



========================================================

function foo() {
	console.log(this.a);
}
var a = 2;
foo.call(null);



========================================================

function foo(a,b) {
	console.log('a: '+ a +' b: '+ b);
}
foo.apply(null, [2,3]);
var bar = foo.bind(null, 2);
bar(3);


========================================================

function foo(a, b) {
	console.log('a: '+ a +' b: '+ b);
}
var wishNull = Object.create(null);
foo.apply(wishNull, [2,3]);

var bar = foo.bind(wishNull, 2);
bar(3);


========================================================

function foo() {
	console.log(this.a);
}
var a = 2;
var o = {a:3, foo: foo};
var p = {a:4};

o.foo();
(p.foo = o.foo)()

-----------------------------------------------------

if(!Function.prototype.softBind) {
	Function.prototype.softBind = function(obj) {
		var fn = this;
		var curried = [].slice.call(arguments, 1);
		
		var bound = function() {
			return fn.apply(
				(!this || this == (window || global))? obj: this.curried.concat.apply(curried, arguments)
			);
		}
		bound.prototype = Object.create(fn.prototype);
		return bound;
	}
}
function foo() {
	console.log('name:' + this.name);
}
var obj = {name: 'obj'},
	obj2 = {name: 'obj2'},
	obj3 = {name: 'obj3'};

var fooOBJ = foo.softBind(obj);
fooOBJ();

obj2.foo = foo.softBind(obj);
obj2.foo();

fooOBJ.call(obj3);
setTimeout(obj2.foo, 10);

----------------------------------------------
function foo() {
	var self = this;
	setTimeout(function() {
		console.log(self.a)
	}, 100)
}
var obj = {
	a: 2
}
foo.call(obj);


数据库：
	可区别度最高的项放最前面，提 高效率
	
	数据类型的一致性(表里面存的和查的数据字段)，数据库会自动做数据类型转化，消耗CPU
	explain
	
	
	为项目重新设置git remote url：
	git remote set-url origin http://gitlab.aspiration-ad.com:8880/wangxin/h5vue.git
	
	
	
Products mentioned: 
10. Botanical Esthé 早安面膜 
11. HABA 魚子醬面膜 
15. Elixir Balancing Bubble 洗顏慕斯 
16. Elixir Revitalizing Care Lifting moisture lotion II 
17. Elixir Day Care Revolution 金管 SPF 50 PA++++
18. Elixir enriched wrinkle cream 
19. HABA Squalane Oil I&II, 30ml 
23. Cle de Peau 隔離霜
27. CECILE MAIA 脱毛膏 200g  important
31. Saborino  / 早安面膜 #酪梨 #葡萄柚 32片入 ¥.1300 / 約NT$.360
32. DHC / 純橄欖護唇膏 5入 /¥.2545 / 約NT$.708
33. KOSE / 雪肌粹美白洗面乳 120g ¥.600 / 約NT$.167
34. Canmake / 肌秘美顏蜜粉餅 ¥.800 / 約NT$.222
35. Canmake / 多功能眼妝底膏 ¥.500 / 約NT$.140
36. Canmake / 小花修容餅 #01  ¥.800 / 約NT$.222
39. Visee / Foggy On Cheeks 烘培腮紅 #RD400 ¥.1500 / 約NT$.417
40. 戀愛魔鏡 / 控制狂防暈眼線液筆 #BK999 ¥.950 / 約NT$.264
41. Softymo / 絲芙蒂 乾濕兩用瞬淨卸妝油 ¥.341 / 約NT$.95
42. ANESSA / 安耐曬 金鑽高效防曬露 SPF50+ PA+++ ¥.2480 / 約NT$.690
43. ALLIE / 高效防曬凝乳 SPF50+ PA+++ ¥.1787 / 約NT$.497
49. KOWA / 蚊蟲止癢液 ¥.462 / 約NT$.129


42. ANESSA / 安耐曬 金鑽高效防曬露 SPF50+ PA+++ ¥.2480 / 約NT$.690
32. DHC / 純橄欖護唇膏 5入 /¥.2545 / 約NT$.708
17. Elixir Day Care Revolution 金管 SPF 50 PA++++
