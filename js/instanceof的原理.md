### 关于instanceof



instanceof是javascript内建的一个二元操作符， 可以用来判断某个对象是否是某个构造函数的实例，如`A instanceof B`可以判断A是否为B的实例。



先来看一段代码：

```javascript
function Foo (name = 'foo') {
    this.name = name;
}

var foo = new Foo();

// #1 true false
console.log(foo instanceof Foo, Foo instanceof Foo); 

// #2 true
console.log(foo instanceof Object); 

// #3 true true
console.log(Object instanceof Function, Function instanceof Object); 
```

第一段输出的第一个结果应该是显而易见的，foo是由Foo构造函数通过new操作符创建出来的，所以应该返回true；第二个输出的结果就可以通过instanceof的原理来解释了，对于表达式`Foo instanceof Foo`,首先会分别读取左值的\_\_proto\_\_（这里用A表示）属性以及右值的prototype属性进行比较，如果结果为false，则继续访问A的\_\_proto\_\_属性重新与右值的prototype属性进行比较，直到读取到原型链末端（未修改的情况下，Object.\_\_proto\_\_.prototype === null）.简而言之，就是查找左值的原型链看是否存在右值的prototype。

这里Foo是一个构造函数，也就是一个函数，所以Foo.\_\_proto\_\_ 的值应该为Function.prototype，显然Foo的原型链上没有Foo.prototype所以返回false。