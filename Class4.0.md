# ES6关键字Class

本文通过以下几个方面来谈谈[ES6中新增的关键字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)Class：

- 背景
- 如何使用Class
- JavaScript是如何实现Class的
- 总结

## 1. 背景

#### 1.1 Class的来历

class由2015年的ES6中正式提出，但是这个关键字最早于一篇JavaScript的草案[JavaScript 2.0 Classes (mozilla.org)](https://www-archive.mozilla.org/js/language/js20-1999-02-18/classes.html)中提出（1999.02），当时因为该关键字过于激进，导致ES4并未将JavaScript2.0中的class关键字合并进去，直到[ES6](https://github.com/tc39/ecma262)草案的提出和通过。

#### 1.2 ES6加入Class的原因

我们在TC39的[GitHub](https://github.com/tc39/ecma262)找到了ES6的提案，在[Stage0阶段关于Class的提案](https://web.archive.org/web/20160729180156/http://wiki.ecmascript.org/doku.php?id=harmony:classes#const)中找到了如下解释：

![image-20201213151123623](https://github.com/ONLY-yours/Picture/raw/main/image-reason-new.png)

##### 翻译：

- `ECMAScript已经拥有了非常完备的功能来定义各种事物的抽象。构造函数、原型、实例这三个的特性足以让实现那些别的语言中类能做到的事。这个稻草人（暂时理解为 功能）的目的不是改变那些语义。相反的，为这些语义提供简洁的声明性表示，能够清楚的表达出程序员的意图而不是底层的命令性机制。`

通俗的来说，Class关键字给我们提供了一种方式，离开那些繁琐的prototype、call，写出逻辑清晰的代码。

## 2. 如何使用Class

#### 2.1 创建类

使用**ES6中的Class来实现类**和**ES5中实现类**的比较：

![image-20201213162233233](https://github.com/ONLY-yours/Picture/raw/main/image-2020.png)

这里我们创建了一个类，含有name和dept两个参数，并且含有一个静态方法fun和一个方法getName。

#### 2.2 通过继承实现子类

使用**extends实现继承**和**ES5中的继承**进行比较：

![image-20201213161227035](https://github.com/ONLY-yours/Picture/raw/main/image.png)

这里我们通过继承父类Employee创建了子类Manager，比父类多一个reports的属性，其余全部继承自父类。

#### 2.3 比较与思考

我们观察上面的子类和父类，使用class创建子类在逻辑上比ES5的方法更加清晰。再比较子类Manager，使用extends继承创建的子类Manager自然继承了其中的属性和方法（包括静态方法），但是ES5中实现继承，不仅需要通过call解决调用的问题，同时静态方法无法直接继承，需要再子类中二次声明才能继承。

但是Class真的再所有地方都优于传统代码吗？其实也不见得，在很多场合下如果只是单纯想使用一次该对象，为何一定要抽象出类来呢？使用字面量对象是不是更加好一些。

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

## 3. JavaScrpit是如何实现Class的

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

## 4. 总结

ES6中的关键字Class本质上还是通过原型链和函数做的，只不过相比于使用ES5写法，class给我们提供了一种写法更加方便、逻辑更加清晰的方法去实现类。

也就如[TC39](https://github.com/tc39/)在ES6[提案](https://web.archive.org/web/20160729180156/http://wiki.ecmascript.org/doku.php?id=harmony:classes#const)上所说的：

- **it’s to provide a terse and declarative surface for those semantics so that programmer intent is expressed instead of the underlying imperative machinery**

- 希望程序员通过class关键字清楚的表达出他们想要的东西，让他们的想法得以表达，而不是还要思考底层的机制。

### 参考内容

[Stack Overflow关于Class关键字的讨论](https://stackoverflow.com/questions/1728984/class-keyword-in-javascript)

[GitHub上TC39的ES6提案库](https://github.com/tc39/ecma262)

[2016年 ES6中关于Class的提案](https://web.archive.org/web/20160729180156/http://wiki.ecmascript.org/doku.php?id=harmony:classes#const)

[ES6新增关键字-MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)

[JavaScript2.0草案](https://www-archive.mozilla.org/js/language/js20-1999-02-18/classes.html)

[类私有域  MDN ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/Private_class_fields)

[JavaScript中7中设计模式](https://medium.com/javascript-in-plain-english/7-javascript-design-patterns-every-developer-should-know-df9c40e7debf)

[JavaScript深入之从原型到原型链](https://github.com/mqyqingfeng/Blog/issues/2)



