## ES6关键字Class

本文通过以下几个方面来谈谈[ES6中新增的关键字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)`Class`

- 背景
- 如何使用Class
- 横向、纵向对比
- JavaScript是如何实现Class的
- Class关键字的限制
- 总结

## 1. 背景

#### 1.1 Class的来历

`Class`由2015年的`ES6`中正式提出，但是这个关键字最早于一篇JavaScript的草案[JavaScript 2.0 Classes (mozilla.org)](https://www-archive.mozilla.org/js/language/js20-1999-02-18/classes.html)中提出（1999.02），当时因为该关键字过于激进，导致ES4并未将JavaScript2.0中的class关键字合并进去，直到[ES6](https://github.com/tc39/ecma262)草案的提出和通过。

#### 1.2 ES6加入Class的原因

我们在TC39的[GitHub](https://github.com/tc39/ecma262)找到了ES6的提案，在[Stage0阶段关于Class的提案](https://web.archive.org/web/20160729180156/http://wiki.ecmascript.org/doku.php?id=harmony:classes#const)中找到了如下解释：

![image-20201213151123623](https://github.com/ONLY-yours/Picture/raw/main/image-reason-new.png)

##### 翻译：

> ECMAScript已经拥有了非常完备的功能来定义各种事物的抽象。构造函数、原型、实例这三个的特性足以让实现那些别的语言中类能做到的事。这个稻草人（暂时理解为 功能）的目的不是改变那些语义。相反的，为这些语义提供简洁的声明性表示，能够清楚的表达出程序员的意图而不是底层的命令性机制。

通俗的来说，`Class`关键字给我们提供了一种方式，离开那些繁的`prototype`、`call`，写出逻辑清晰的代码。

## 2. 如何使用Class

#### 2.1 创建类

使用**ES6中的Class来实现类**和**ES5中实现类**的比较：

![image-20201213162233233](https://github.com/ONLY-yours/Picture/raw/main/image-2020.png)

这里我们创建了一个类，含有name和dept两个参数，并且含有一个静态方法fun和一个方法getName。

#### 2.2 通过继承实现子类

使用**extends实现继承**和**ES5中的继承**进行比较：

![image-20201213161227035](https://github.com/ONLY-yours/Picture/raw/main/image.png)

这里我们通过继承父类`Employee`创建了子类`Manager`，比父类多一个reports的属性，其余全部继承自父类。

## 3. 横向、纵向比较

传统的[OO](https://en.wikipedia.org/wiki/Object-oriented_programming)语言（java，c++等等），都包含例如类、构造函数、继承、静态方法等等。他们通过这些来实现面向对象的功能，那么`JavaScript`是如何做的呢？

#### 3.1 JavaScript和Java进行横向比较（以[Java](https://en.wikipedia.org/wiki/Java_(programming_language))为例）

*1. 类的概念（**Class**）：*

- JavaScript同java类似，都采用了class来声明一个类。

*2. 构造函数（**constructor**）：*

- JavaScript使用constructor方法当作构造函数，而java采用类名作为构造函数。

*3. 子类（***extends**）：

- JavaScript同java一致，都使用extends来继承。

*4. 静态方法（**static**）：*

- ES6中JavaScript同java一致，使用extends创建子类自然就将父类的静态方法继承过来。ES5则需要手动声明指向。

*5. 类的属性（**int、double、String**等）：*

- JavaScript得益于类型自动推断，不需要显示的声明类的属性。

#### 3.2 ES6和ES5的纵向比较

我们观察上面（2.0如何使用Class）的子类和父类，使用`class`创建子类在逻辑上比ES5的方法更加清晰。再比较子类`Manager`，使用`extends`继承创建的子类`Manager`自然继承了其中的属性和方法（包括静态方法），但是ES5中实现继承，不仅需要通过call解决调用的问题，同时静态方法无法直接继承，需要在子类中二次声明才能继承。

但是`Class`真的在所有地方都优于传统代码吗？其实也不见得，在很多场合下如果只是单纯想使用一次该对象，为何一定要抽象出类来呢？使用字面量对象是不是更加好一些。

```javascript
//字面量对象
let person ={
    name : "Jason",
    age : 18,
    adress : "hangzhou",
    phone : "10086",
    getPhone : function(){
        return this.phone
    }
}
```

## 4. JavaScrpit是如何实现Class的

我们都知道ES5中实现类的方法是通过[原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)来实现的（[详解JavaScript原型链](https://github.com/mqyqingfeng/Blog/issues/2)），我们使用ES5的写法，实现一个Employee的实例：

```javascript
//ES5 
let employ =new Employee(1,2)

employ.__proto__.constructor === Employee  //true
employ.__proto__ === Employee.prototype   //true
Employee.prototype.constructor === Employee  //true
```

这是没有问题的，```__proto__```指针指向构造函数的原型，我们用ES6的class再来判断一遍：

```javascript
//ES6使用class
let employ =new Employee(1,2)

employ.__proto__.constructor === Employee  //true
employ.__proto__ === Employee.prototype   //true
Employee.prototype.constructor === Employee  //true
```

两者的结果是相同的！只不过使用函数创建的对象，```__proto__```指针指向构造函数，使用class关键字创建的对象，```__proto__```指针指向类。

我们再来看这串代码：

```javascript
console.log(typeof(Employee));	//function
```

我们将class放入，竟然返回的是function，也就是class关键字本质上还是一个函数，使用class创建出来的类还是基于函数和原型链。

![image](https://github.com/ONLY-yours/Picture/raw/main/image-20201203164243589.png)

## 5. Class关键字的限制

既然本质上`Class`仍然是一个函数，他做的事情又是将ES5的写法更加精简，那么自然就会带来一些限制。

#### 5.1 灵活性

假设现在有一个需求，在一个`Manager`对象中加入一个方法，如果使用ES5的方法只需要在`prototype`上加入方法即可

```javascript
let manager1 = new Manager(1,2,3)
manager1.prototype.FunctionName = function(){
    /*写自己的逻辑*/
}
```

那么使用`class`该怎么办呢？

加在`Manager`中？或是加在`Employee`中？

如果这个方法只是这个特殊的对象拥有的呢？而不想让所有`Manager（Employee）`都有这个方法的话，修改`Class`中的方法看起来似乎不那么正确。如果单纯为这个对象创建一个子类，则更加繁琐，这与`Class`关键字的提出是为了更加方便的目的背道而驰，在这种场景下`Class`显得灵活性就不如`ES5`的`prototype`了。

#### 5.2 类声明不可提升

[函数声明](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function)具有可提升性，虽然类本质上是函数，但是类的声明却不可提升，也就是说你一定要先声明类，才能创建对象。

```javascript
let a = new ES6_Practice()    //报错
let b = new ES5_Practice()    //成功

class ES6_Practice{
}

function ES5_Practice(){
}
```

#### 5.3 无法重写类

当我们在特定时刻想要重写某个函数方法，只需要重新定义方法即可，但是类却无法重写，如果出现两个相同的类名，则会报错语法错误（[SyntaxError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)）。

```javascript
class ES6_Practice{
}
class ES6_Practice{   //报错SyntaxError
	console.log("new class");
}

function ES5_Practice(){  
}

function ES5_Practice(){	//没问题,重写成功
  console.log("new function");
}
```

## 6. 总结

ES6中的关键字`Class`本质上还是通过原型链和函数做的，虽然仍然存在一些问题（灵活性不足等），但相比于使用ES5写法，`Class`给我们提供了一种写法更加方便、逻辑更加清晰的方法去实现类。

也就如[TC39](https://github.com/tc39/)在ES6[提案](https://web.archive.org/web/20160729180156/http://wiki.ecmascript.org/doku.php?id=harmony:classes#const)上所说的：

>**it’s to provide a terse and declarative surface for those semantics so that programmer intent is expressed instead of the underlying imperative machinery**

 > 希望程序员通过`Class`关键字清楚的表达出他们想要的东西，让他们的想法得以表达，而不是还要思考底层的机制。

### 参考内容

[Stack Overflow关于Class关键字的讨论](https://stackoverflow.com/questions/1728984/class-keyword-in-javascript)

[GitHub上TC39的ES6提案库](https://github.com/tc39/ecma262)

[2016年 ES6中关于Class的提案](https://web.archive.org/web/20160729180156/http://wiki.ecmascript.org/doku.php?id=harmony:classes#const)

[ES6新增关键字-MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)

[JavaScript2.0草案](https://www-archive.mozilla.org/js/language/js20-1999-02-18/classes.html)

[类私有域  MDN ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/Private_class_fields)

[Java-wiki](https://en.wikipedia.org/wiki/Java)

[JavaScript中7中设计模式](https://medium.com/javascript-in-plain-english/7-javascript-design-patterns-every-developer-should-know-df9c40e7debf)

[JavaScript深入之从原型到原型链](https://github.com/mqyqingfeng/Blog/issues/2)

[OO语言-wiki](https://en.wikipedia.org/wiki/Object-oriented_programming)

