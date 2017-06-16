## Es6概览

### 一、let和const
Es6中有块级作用域的概念。
在es6之前只有两种声明方式var 和function。
在es6中新增了四种声明方式let、const、class、import。
Var
Function
在es6，let和const都应用在块级作用域中，什么是块级作用域，由｛｝包裹的是块级作用域。
```
{
  let a = 1
  let a = 2//Uncaught SyntaxError: Identifier 'a' has already been declared
  const b = 2
  b=3//TypeError: Assignment to constant variable.
  var c = 3
}

a // ReferenceError: a is not defined.
b // ReferenceError: b is not defined.
c // 3
```
以上就是块级作用域。let声明的变量在块级作用域之外不乏被访问。由let和const声明的变量无法在块之外访问。var声明的变量则可以。const是常量，只能在声明的时候赋值。let声明不允许重复声明。

for循环计数器很适合食用let命令，为什么？（因为一般来说我们希望，在for循环计数器的变量在这个for循环块内起作用不影响到外部的全局变量。）
```
var a=[];
for(var i=0;i<10;i++){
a[i]=function(){
console.log(i);
}
}
a[1]();//10
a[9]();//10
```
这是一个常见的面试题吧。当我们用let来替代var声明符时。
```
var a= [];
for(let i=0;i<10;i++){
a[i]=function(){
console.log(i);
}
}
a[1]();//1
a[9]();//9
```
我们可以看到我们想要的结果，这是之前用var声明符办不到的。因为变量声明只在块级作用域内有效。i用let声明，只在本轮循环有效，下一次循环其实i已经是另一个新的变量了

因为let和const的变量声明不会被提前，就又产生了一种现象叫暂时性死区，即在块级作用域中let和const变量声明之前都是不能使用这个变量的。

const常量保存一个对象的时候，或者说保存一个引用量的时候，其保存的不是一个引用量的值，而是这个引用量的内存地址。所以
```
const a={};
const b={};
a.b=11111111；//当这个常量是一个对象的时候，这个常量其实还是可以赋值的。
a={}//但是把它复制给另一个值的时候，就没有法了，因为这会改变这个常量所指的内存地址。
```
### 二、字符串
ES6 字符串相关的知识点最重要的是模版字符串，然后是一些没啥用的新增API。

在ES6以前，字符串用单引号或者双引号包裹，这种字符串最大的缺点是不能换行，书写时换行会报错
```
let str = "aaa
          aaa"

// Uncaught SyntaxError: Invalid or unexpected token
```
在ES6中，新增了一种字符串表示方式，用反引号 ` （键盘上数字1左侧的按键）包裹，允许插入变量或简单的JavaScript表达式，也允许换行书写
```
let str = `asd
	  asd
	  ads`

let x = 1;
let y = 2;

// $后面的花括号里的字符串会被js解析器认为是JavaScript语句，这点和jsx很像，但是只能写JavaScript表达式。
`${x} + ${y} = ${x + y}`
// "1 + 2 = 3"

`${x} + ${y * 2} = ${x + y * 2}`
// "1 + 4 = 5"

let obj = {x: 1, y: 2};
`${obj.x + obj.y}`
// 3
```
ES6为字符串添加了lterator接口，使字符串可以被for...of循环遍历。
```
for (let x of 'andy') {
  console.log(x)
}
//‘a’
//’n’
//‘d’
//‘y’
```
### 三、数字扩展
ES6在Number对象上，新提供了 **Number.isFinite()** 和
 **Number.isNaN()** 两个方法。

Number.isFinite()用来检查一个数值是否为有限的（finite）。

Number.isNaN()用来检查一个值是否为NaN。
它们与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，非数值一律返回false。
```
isNaN(NaN) // true
isNaN("NaN") // true
Number.isNaN(NaN) // true
Number.isNaN("NaN") // false
```
js小数运算不准确的原因是因为小数在转换为二进制的时候是无限循环小数。
0.1+0.2//0.30000000000000004

ES6新增了一个常量 Number.EPSILON 来表示小数运算时的误差范围

`Number.EPSILON//2.220446049250313e-16`

ES6将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。
```
Number.parseInt(1.234)//1
Number.parseFloat(1.234)//1.234
```
Number.isInteger()用来判断一个值是否为整数。需要注意的是，在JavaScript内部，整数和浮点数是同样的储存方法，所以3和3.0被视为同一个值。
```
Number.isInteger(3)//true
Number.isInteger(3.0)//true
```
JavaScript能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。

```
Math.pow(2, 53) // 9007199254740992
Math.pow(2, 53) === Math.pow(2, 53) + 1// true
```
ES6引入了Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER这两个常量，用来表示这个范围的上下限。
```
Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内。
Number.isSafeInteger('a') // false
Number.isSafeInteger(9007199254740990) // true
Number.isSafeInteger(9007199254740992) // false
```

ES7中新增了一个指数运算符（**）
```
2 ** 2 // 4
2 ** 3 // 8
```
### 四、数组
ES6中，数组新增了一系列好用的API
还有一个非常重要而且常用的语法：扩展运算符 **...**


扩展运算符写作三个点（…），可以把一个可遍历的对象（只要有iterator接口就可以）解开，转为用逗号分隔的序列
```
console.log(...[1, 2, 3])
// 1 2 3
1,2,3
console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

[...document.querySelectorAll('div')]
// [<div>, <div>, <div>]

[...'abc']
// ["a", "b", “c"]
```
Array.from方法用于将可遍历的对象转化为数组
```
// NodeList对象
let ps = document.querySelectorAll('p')
console.log(Array.from(ps))
// [p, p]

// arguments对象
function foo() {
  console.log(Array.from(arguments))
}
foo(1, 2, 3)
// [1, 2, 3]
```

Array.prototype.find方法用于查找数组中第一个符合要求的值，
```
[1, 4, -5, 10].find(function (x) {
  return x > 3
 })
// 4
```
注意返回第一个满足要求的值，如果想知道这个值对应的位置，可以把find换成findIndex


Array.prototype.includes方法用来判断数组中是否含有某个值，含有就返回true，否则返回false
```
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
```

### 五、函数
ES6中我们可以使用“箭头”（=>）定义函数。
```
function(arguments){
	return ;
}
var f = v => v

(num1, num2) => {
  let num3 = 3
  return num1 + num2 + num3
}
```
并且箭头函数内部的this被绑定为它定义时所在的对象，而不是随着调用方式不同而改变。箭头函数中取消了arguments对象，是因为ES6中有了更好的替代方式：扩展运算符。如果扩展运算符写在函数的参数中，则称作rest参数，是扩展运算符的逆运算
```
let foo = (...values) => console.log(values)
foo(1, 2, 3)
// [1, 2, 3]
```

注意这种默认值的声明方式与let类似，函数代码块中不能用let或const再次声明同一个参数
```
let foo = (x = 5) => {
  let x = 1  // error
  const x = 2  // error
}
```
### 六、对象
```
var foo = 'bar';
var baz = {foo};
baz // {foo: "bar"}
// 等同于
var baz = {foo: foo};
```
es6允许直接在对象中使用变量，使用变量时,属性名为变量名，属性值为变量值。

除了属性，方法也可以简写。
```
var o = {
  method() {
    return "Hello!";
  }
};

// 等同于

var o = {
  method: function() {
    return "Hello!";
  }
};
```

Object.assign 方法用于对象的合并，接收的参数是任意个对象，会依次把第2，3，4...个对象
合并到第一个对象上，如果有重复的属性名，后来的会覆盖先前的。
```
let target = { a: 1, b: 1 }
let source1 = { b: 2, c: 2 }
let source2 = { c: 3 }
Object.assign({b:1,a:{b:1}},{a:{b:{}}})
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

Object.is 方法用于判断两个值是否相等
以前我们判断两个值相等可以用==和===
不过==会发生隐式转换，===无法判断NaN
Object.is与===不同之处只有两个：一是+0不等于-0，二是NaN等于自身。
```
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```


### 七、Map和Set 
set数据结构类似于数组，但是里面不会有任何相同的值。
这让我第一时间想到的就是，我们的数组去重，再也不用写那么多遍历了。
```
const theset = new set();//set本身是一个构造器，构造器需要使用new操作符。
[1,2,3,4,5,5,6,6,7,7,7,7].each((item)=theset.add>(item));
for (let i of theset) {
  console.log(i);
}//1,2,3,4,5,6,7


New set(…[1,2,3,4,5,5,5,5,5])
```
set的方法一共有两种，一种是遍历方法，另一种是操作方法。

set.add()在set结构中添加一个值

set.has()判断set结构中是否有这个值
```
set.delete()删除指定set值
set.clear()//清除所有
```
因此使用 Set 可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）。
```
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

Map 

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。
Object 结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，属性名可以是一个一个变量。

### 八、Symbol
ES6引入了一种新的原始数据类型Symbol，表示独一无二的值

它是JavaScript语言的第七种数据类型，前六种是：

Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）

Symbol值与字符串类似，可以当做对象的属性名
因为Symbol都是独一无二的，所以可以保证不会与其他属性名产生冲突

Symbol值通过Symbol函数生成，Symbol函数可以接受一个字符串作为参数，表示对Symbol的描述
主要用于调试的时候方便区分不同的Symbol值
```
let aaa = Symbol('a')
let bbb = Symbol('b')

let obj = {
  [aaa]: 'Hello!',
  [bbb]: 'World!'
}

console.log(obj)
// Object {Symbol(a): "Hello!", Symbol(b): "World!"}
obj[aaa]
// "Hello!"
obj[bbb]
// "World!"
```

### 九、解构赋值
数组的解构赋值很好理解，就是按照对应位置，对变量赋值：
```
let [a, b, c] = [1, 2, 3]
//相当于
let a = 1
let b = 2
let c = 3
```
如果变量对应不上值，这个变量就是undefined，如果一个值对应不到变量，那这个值被忽略
```
[a, b, c] = [1, 2]
// a,b为1，2   c由于匹配不到为undefined
[a, b] = [1, 2, 3]
// a,b为1，2   3由于没有变量接收被忽略
```

解构赋值允许指定默认值，也允许使用rest参数(...)来接收不确定个数的参数：
```
let [firstName = "John", lastName = "Doe"] = []
let [a, b, ...rest] = [1, 2, 3, 4, 5]
a    // 1
b    // 2 
rest // [3, 4, 5]

Arguments
function(…rest){
	console.log(…rest);

}
```
对象的解构赋值与数组类似，只有在属性名对应相同的情况下，右侧属性的值会被赋值给左侧对应属性的值
```
let {firstName: name, lastName} = {firstName: "John", lastName: "Doe"}
name      // "John"
lastName  // "Doe"
firstName // Uncaught ReferenceError: firstName is not defined
let {"0": a, "1": b} = {"0": 1, "1": 2}
a     // 1
b     // 2
```

### 十、iterator接口
在ES6以前，我们想遍历一个数组一般有三种方式：

for 循环
for ... in 语句
Array.prototype.forEach 方法
三种方式都有不同程度的缺陷

for循环写法繁琐，需要指定循环次数不超过数组长度，否则就会有对应的报错

for ... in 语句遍历出的是键值，需要做一些转换才能拿到数据，而且遍历是无序的

forEach 方法是个不错的方式，不过它不能中途使用break跳出循环，也不能return终止外层函数

基于这些缺陷，ES6新增了 for ... of 的语法，用于遍历数组，对象，字符串，或者是Set，Map等数据结构
for ... of 遍历依赖 iterator 接口，只有部署了 iterator 接口的数据类型才可以被遍历

### 十一、promise
Promise对象最早是由社区提出的一种解决异步编程的方案。后台像angular这些框架都支持了这个方案。在es6中也引入了这个方案。

Promise是一个构造函数,我们可以用new关键字生成一个promise实例来使用
```
let promise = new Promise((resolve, reject) => {
        //做一些异步操作
        setTimeout(() => {
            console.log('success')
            resolve('promise is success')
        }, 2000)
    })
```
Promise构造函数接受一个函数作为参数,在这个函数内执行一些异步操作
这个函数拥有两个参数:resolve和reject,这两个都是函数,js引擎会帮你传入
在函数内部调用他们的时候分别代表对外声明异步操作已经成功(resolve)或失败(reject)
```
settimeout(function(){
},1000)

promise.then(function(){},function(){})



setTimeout(() => {
  return 1
}, 2000)
```
这是一个简单的回调方式处理异步操作的结果,但是回调函数被外层的定时器包裹

我们没法简单的拿到回调函数的返回值,这也是回调函数最大的缺陷

所以promise通过resolve和reject方法,对外传递一个可以轻易访问到信息

如果我们需要拿到并处理promise内部resolve的信息,需要使用then方法:
```
let promise = new Promise((resolve, reject) => {
        //做一些异步操作
        setTimeout(() => {
            console.log('success')
            resolve('promise is success')
        }, 2000)
    })

promise.then(x => console.log(x))
// promise is success
```
promise实例上的then方法是promise里最常用的方法,它接受一个函数为参数,这个函数的第一个参数就是这个promise内部resolve出来的值,从而我们可以在这个函数内部获取和使用这个值

另一个常用方法是catch,如果这个promise异步操作出了问题

我们会在函数内部调用reject方法传递出去错误信息,代表promise出错了

这时候应该使用catch方法来处理错误信息:
```
let promise = new Promise((resolve, reject) => {
        //做一些异步操作
        setTimeout(() => {
            console.log('error')
            reject('promise is error')
        }, 2000)
    })

promise.then(x => console.log(x))
// 报错 Uncaught (in promise) promise is success

promise.catch(x => console.log(x)
// 不报错 输出 promise is error
```
其实如果你给then方法传入两个函数,那么第二个函数也是可以捕获到这个错误的

catch方法只是then的一个别名,不过为了代码清晰易读

我们最好都是用catch,而不是给then传入两个参数
现在我们有then方法处理执行成功的promise,用catch处理出错的promise

显然需要一种无论成功还是出错都能处理的方法,就像try...catch中的finally

promise也提供了一个finally方法,用法与then,catch完全相同

无论promise成功还是失败都会执行finally中的回调函数

除了这四种promise实例上的方法以外,js还原生提供了Promise.resolve()和Promise.reject()方法
注意不是promise内部的resolve,reject方法,不要混淆

这两个方法可以简单的把一个异步操作包装成promise:
```
Promise.resolve('success')
// 等价于
new Promise((resolve, reject) => resolve('success'))

Promise.reject('error')
// 等价于
new Promise((resolve, reject) => reject(‘error'))
```
### 十二、class
在es5中我们使用构造函数来定义并生成新的对象，在es6中引入了类的概念。class只是一个语法糖。
```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```
这里是es6的写法。
```
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```
我们其实可以发现事实上，类的所有方法都定义在类的prototype属性上面。

class不存在变量提升，所以会有变量死区这个概念。


Class表达式
与函数一样，类也可以使用表达式的形式定义。
```
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```
上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是MyClass而不是Me，Me只在Class的内部代码可用，指代当前类。

私有方法是常见需求，但 ES6 不提供，只能通过变通方法模拟实现。
一种做法是在命名上加以区别。
```
class Widget {

  // 公有方法
  foo (baz) {
    this._bar(baz);
  }

  // 私有方法
  _bar(baz) {
    return this.snaf = baz;
  }

  // ...
}
```
上面代码中，_bar方法前面的下划线，表示这是一个只限于内部使用的私有方法。但是，这种命名是不保险的，在类的外部，还是可以调用到这个方法。


另一种方法就是索性将私有方法移出模块，因为模块内部的所有方法都是对外可见的。
```
class Widget {
  foo (baz) {
    bar.call(this, baz);
  }

  // ...
}

function bar(baz) {
  return this.snaf = baz;
}


class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```
上面代码中，constructor方法和toString方法之中，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。

子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。



如果子类没有定义constructor方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何一个子类都有constructor方法。
constructor(...args) {
  super(...args);
}

super这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

第一种情况，super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。
```
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```
上面代码中，子类B的构造函数之中的super()，代表调用父类的构造函数。这是必须的，否则 JavaScript 引擎会报错。

第二种情况，super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
```
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}
let b = new B();
```
上面代码中，子类B当中的super.p()，就是将super当作一个对象使用。这时，super在普通方法之中，指向A.prototype，所以super.p()就相当于A.prototype.p()。
ES6 规定，通过super调用父类的方法时，super会绑定子类的this。
```
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```
类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。
```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```
上面代码中，Foo类的classMethod方法前有static关键字，表明该方法是一个静态方法，可以直接在Foo类上调用（Foo.classMethod()），而不是在Foo类的实例上调用。

如果在实例上调用静态方法，会抛出一个错误，表示不存在该方法。

父类的静态方法，可以被子类继承。
```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod(); // 'hello'
```
上面代码中，父类Foo有一个静态方法，子类Bar可以调用这个方法。

```
{value:++x,done:false}
{value:undefined,done:true}

p={} {}==o
q=o
```
React-native原理概览
react——Virtual DOM和diff算法

react-native——原生和js之间的交互


ios——objc和js之间的交互
ios——JavaScript Core

Jsbridge

```
JSContext *context = [[JSContext alloc] init];  
JSValue *jsVal = [context evaluateScript:@"21+7"];  
int iVal = [jsVal toInt32]; 
```

## ReactElement和ReactClass

### ReactElement 
由React.createElement 和jsx创建 
ReactElement分为 DOM Element 和 Component
#### Elements 两类： 
##### DOM Elements实例
```
{
  type: 'button',
  props: {
    className: 'button button-blue',
    children: {
      type: 'b',
      props: {
        children: 'OK!'
      }
    }
  }
}
```

##### Component Elements 
当节点的type属性为一个函数或一个类时，它代表自定义的节点。 

Component Elements实例
```
class Button extends React.Component {
  render() {
    const { children, color } = this.props;
    return {
      type: 'button',
      props: {
        className: 'button button-' + color,
        children: {
          type: 'b',
          props: {
            children: children
          }
        }
      }
    };
  }
}

// Component Elements
{
  type: Button,
  props: {
    color: 'blue',
    children: 'OK!'
  }
}
```
