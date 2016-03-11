# 对象

## 对象的使用和属性

数字的字面量不能当作对象使用，这是 JS 解析器的一个错误，它试图将点操作符解析为浮点数字面值的一部分。

```
2.toString(); // Error
```

我们可以作如下转换

```
2..toString(); // 第二个点号可以正常解析
2 .toString(); // 注意点号前面的空格
(2).toString(); // 先被计算
```

### 访问对象

访问对象属性的两种方式，点操作符和中括号操作符

```
var foo = {name: 'kitten'}
foo.name; // kitten
foo['name']; // kitten

var get = 'name';
foo[get]; // kitten

foo.1234; // SyntaxError
foo['1234']; // works
```

中括号操作符在下面的场景依旧有效

- 动态设置属性
- 属性名不是一个有效的变量名（如属性名中包含空格，或者是JS关键词）

### 删除属性

删除属性的唯一方法是使用 delete 操作符，设置属性为 undefine 或者 null 不能真正地删除属性，仅仅只是移除属性和值的关联。

```
 var obj = {
    bar: 1,
    foo: 2,
    baz: 3
};
obj.bar = undefined;
obj.foo = null;
delete obj.baz;

// bar undefine
// foo null
for(var i in obj) {
    if (obj.hasOwnProperty(i)) {
        console.log(i, '' + obj[i]);
    }
}
```
 
### 属性名和语法
 
```
var test = {
    'case': 'I am a keyword so I must be notated as a string',
    delete: 'I am a keyword too so me' // 出错：SyntaxError
};
```

delete 是 JS 语言的关键词，为了在低版本的 JS 引擎下运行，必须使用字符串字面量声明。

## 原型

JS 使用 prototype 原型模型。和传统类继承模型不一样。  

原型链继承方式

```
function Foo() {
    this.value = 42;
}
Foo.prototype = {
    method: function() {}
};

function Bar() {}

// 设置Bar的prototype属性为Foo的实例对象
Bar.prototype = new Foo();
Bar.prototype.foo = 'Hello World';

// 修正Bar.prototype.constructor为Bar本身
Bar.prototype.constructor = Bar;

var test = new Bar() // 创建Bar的一个新实例

// 原型链
test [Bar的实例]
    Bar.prototype [Foo的实例] 
        { foo: 'Hello World' }
        Foo.prototype
            {method: ...};
            Object.prototype
                {toString: ... /* etc. */};
```
 
note  
注意: 简单的使用 Bar.prototype = Foo.prototype 将会导致两个对象共享相同的原型。 因此，改变任意一个对象的原型都会影响到另一个对象的原型，在大多数情况下这不是希望的结果。  
 
### 属性查找
 
当查找一个对象的属性时，JS 会向上遍历原型链，直到找到给定名称的属性为止。  
 
到查找到达原型链的顶部，也就是 Object.prototype，但是仍然没有找到指定的属性，就会返回 undefine。  
 
### 原型属性
 
```
function Foo() {}
Foo.prototype = 1; // 无效
```
当原型属性用来创建原型链时，可以赋任何类型的值。  
但是原子类型的值会被忽略，而将对象赋值给原型属性，将会动态地创建原型链。

### 扩展内置类型的原型

扩展 Object.prototype 或者其他内置类型的原型对象。  
这种技术被称为 monkey patching，会破坏封装性。 

### Sunmmary

在写复杂的 JavaScript 应用之前，充分理解原型链继承的工作方式是每个 JavaScript 程序员必修的功课。 要提防原型链过长带来的性能问题，并知道如何通过缩短原型链来提高性能。 更进一步，绝对不要扩展内置类型的原型，除非是为了和新的 JavaScript 引擎兼容。

## hasOwnProperty 函数

判断一个对象是否包含自定义属性而不是原型链上的属性。

hasOwnProperty 是 JS 中唯一一个处理属性但是不查找原型链的函数。

```
// 修改Object.prototype
Object.prototype.bar = 1; 
var foo = {goo: undefined};

foo.bar; // 1
'bar' in foo; // true

foo.hasOwnProperty('bar'); // false
foo.hasOwnProperty('goo'); // true
```

### hasOwnProperty 作为属性

```
var foo = {
    hasOwnProperty: function() {
        return false;
    },
    bar: 'Here be dragons'
};

foo.hasOwnProperty('bar'); // 总是返回 false

// 使用其它对象的 hasOwnProperty，并将其上下文设置为foo
({}).hasOwnProperty.call(foo, 'bar'); // true
```

### Summary

当检查对象上某个属性是否存在时，hasOwnProperty 是唯一可用的方法。 同时在使用 for in loop 遍历对象时，推荐总是使用 hasOwnProperty 方法， 这将会避免原型对象扩展带来的干扰。

## for in 循环

```
// 修改 Object.prototype
Object.prototype.bar = 1;

var foo = {moo: 2};
for(var i in foo) {
    console.log(i); // 输出两个属性：bar 和 moo
}
```

### 使用 hasOwnProperty 过滤

```
// foo 变量是上例中的
for(var i in foo) {
    if (foo.hasOwnProperty(i)) {
        console.log(i);
    }
}
```

### Summary

推荐总是使用 hasOwnProperty。不要对代码运行的环境做任何假设，不要假设原生对象是否已经被扩展了。

# 函数

## 函数声明

```
foo(); // 正常运行，因为foo在代码运行前已经被创建
function foo() {}
```

上面的方法会在执行前被 解析(hoisted)，因此它存在于当前上下文的任意一个地方， 即使在函数定义体的上面被调用也是对的。

## 函数赋值表达式

```
foo; // 'undefined'
foo(); // 出错：TypeError
var foo = function() {};
```

由于 var 定义了一个声明语句，对变量 foo 的解析是在代码运行之前，因此 foo 变量在代码运行时已经被定义过了。  

但是由于赋值语句只在运行时执行，因此在相应代码执行之前， foo 的值缺省为 undefined。  

转换如下：

```
var foo;
foo; // 'undefined'
foo(); // 出错：TypeError
foo = function() {};
```

## 命名函数的赋值表达式

```
var foo = function bar() {
    bar(); // 正常运行
}
bar(); // 出错：ReferenceError
```

bar 函数声明外是不可见的，这是因为我们已经把函数赋值给了 foo； 然而在 bar 内部依然可见。这是由于 JavaScript 的 命名处理 所致， 函数名在函数内总是可见的。  

注意:在IE8及IE8以下版本浏览器bar在外部也是可见的，是因为浏览器对命名函数赋值表达式进行了错误的解析，解析成两个函数 foo 和 bar。  

## this 的工作原理

JS 和其它语言的 this 机制完全不同。

### 全局范围内

```
this
```

当全局范围内使用 this，它将会指向全局对象。

### 函数调用

```
foo();
```

这里的 this 也指向全局对象。  
在浏览器中，这个全局对象是 window。  

### 方法调用

```
test.foo();
```

this 指向 test 对象。  

### 调用构造函数

```
new foo();
```

this 指向新创建的对象。

### 显式设置 this

```
function foo(a, b, c) {}

var bar = {};
foo.apply(bar, [1, 2, 3]); // 数组将会被扩展，如下所示
foo.call(bar, 1, 2, 3); // 传递到foo的参数是：a = 1, b = 2, c = 3
```

当使用 Function.prototype 上的 call 或者 apply 方法时，函数内的 this 将会被 显式设置为函数调用的第一个参数。  

关于 call 和 apply 的理解可以查看如下代码

```
function add(a, b){console.dir(this);}

function sub(a, b){console.dir(this);}

add(1,2);
"Window"

sub(1,2);
"Window"

add.call(sub, 1, 2);
"sub(a, b)"

sub.apply(add, [1, 2]);
"add(a, b)"

作者：杨佰
链接：https://www.zhihu.com/question/20289071/answer/62582198
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 常见的误解

直接调用函数时，this 指向全局对象

```
Foo.method = function() {
    function test() {
        // this 将会被设置为全局对象（译者注：浏览器环境中也就是 window 对象）
    }
    test();
}
```

为了在 test 中获取对 Foo 对象的引用，我们需要在 method 函数内部创建一个局部变量指向 Foo 对象。

```
Foo.method = function() {
    var that = this;
    function test() {
        // 使用 that 来指向 Foo 对象
    }
    test();
}
```

### 方法的赋值表达式

```
var test = someObject.methodTest;
test();
```

test 就像一个普通的函数被调用；因此，函数内的 this 将不再被指向到 someObject 对象。

## 闭包和引用

闭包是 JavaScript 一个非常重要的特性，这意味着当前作用域总是能够访问外部作用域中的变量。 

### 模拟私有变量

```
function Counter(start) {
    var count = start;
    return {
        increment: function() {
            count++;
        },

        get: function() {
            return count;
        }
    }
}

var foo = Counter(4);
foo.increment();
foo.get(); // 5
```