---
title: Proxy和Object.defineProperty
comments: true
date: 2018-07-10 22:01:28
tags: 
- Proxy
- defineProperty
---



## **Proxy与Object.defineProperty**

### **一、Proxy**

let proxy = new Proxy(target, handler);

*tgarget*：要代理的目标对象。（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。

*handler*：定义拦截行为的配置对象（也是一个对象，其内部的属性均为执行操作的函数）。

**1、**set(target, key, value, receiver)  拦截对象属性的设置

*target*：要代理的目标对象。

*key*：要设置的属性名。

*value*：要设置的属性值。

*receiver*：proxy实例（可选参数，一般不用）。

```js
function Monster() {
  this.eyeCount = 4;
}

const handler1 = {
  set(obj, prop, value) {
    if ((prop === 'eyeCount') && ((value % 2) !== 0)) {
      console.log('Monsters must have an even number of eyes');
    } else {
      return Reflect.set(...arguments);
    }
  }
};

const monster1 = new Monster();
const proxy1 = new Proxy(monster1, handler1);
proxy1.eyeCount = 1;
// expected output: "Monsters must have an even number of eyes"

console.log(proxy1.eyeCount);
// expected output: 4
```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710114517347.png %}

**2、**get(target, key, receiver) 拦截对象属性的读取

*target*：要代理的目标对象。

*key*：属性名。

*receiver：proxy*实例（可选参数，一般不用）

```js
var p = new Proxy({}, {
  get: function(target, prop, receiver) {
    console.log("called: " + prop);
    return 10;
  }
});

console.log(p.a); // "called: a"
                  // 10
```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710114540710.png %}

**3、**has(target, Key) 判断对象是否具有某个属性。

*target*：要代理的目标对象。

*key*：要设置的属性名。

```js
const handler1 = {
  has(target, key) {
    if (key[0] === '_') {
      return false;
    }
    return key in target;
  }
};

const monster1 = {
  _secret: 'easily scared',
  eyeCount: 4
};

const proxy1 = new Proxy(monster1, handler1);
console.log('eyeCount' in proxy1);
// expected output: true

console.log('_secret' in proxy1);
// expected output: false

console.log('_secret' in monster1);
// expected output: true
```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710114558303.png %}

**4、**apply(target, thisArgs, args) 拦截函数的调用、call和apply操作

*target*：目标对象。

*thisArgs*：目标对象的上下文对象（this）。

*args*：目标对象的参数数组。

```js
function sum(a, b) {
  return a + b;
}

const handler = {
  apply: function(target, thisArg, argumentsList) {
    console.log(`Calculate sum: ${argumentsList}`);
    // expected output: "Calculate sum: 1,2"

    return target(argumentsList[0], argumentsList[1]) * 10;
  }
};

const proxy1 = new Proxy(sum, handler);

console.log(sum(1, 2));
// expected output: 3
console.log(proxy1(1, 2));
// expected output: 30
```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710114612270.png %}

**5、**construct(target, args，newTarget) 拦截new命令

*target*：目标对象。

*args*：构造函数的参数列表。

*newTarget*：创建实例对象时，new命令作用的构造函数（下面例子的p）

```js
function monster1(disposition) {
  this.disposition = disposition;
}

const handler1 = {
  construct(target, args) {
    console.log('monster1 constructor called');
    // expected output: "monster1 constructor called"

    return new target(...args);
  }
};

const proxy1 = new Proxy(monster1, handler1);

console.log(new proxy1('fierce').disposition);
// expected output: "fierce"
```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710114625044.png %}

**6、**this问题

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710114641008.png %}

### **二、Object.defineProperty(obj, prop, descriptor)**

Object.defineProperty()的作用就是直接在一个对象上定义一个新属性，或者修改一个已经存在的属性

参数说明：

​	obj：必需。目标对象 

​	prop：必需。需定义或修改的属性的名字

​	descriptor：必需。目标属性所拥有的特性

返回值：

​	传入函数的对象。即第一个参数obj

属性：

```
configurable：表示该属性能否通过delete删除，能否修改属性的特性或者能否修改访问器属性，默认为false。当且仅当该属性的configurable为true时，才能实现上述行为。

enumerable：表示该属性是否可以枚举，即可否通过for..in访问属性。默认为false。

get：在读取属性时调用的函数，默认值为undefined。

set：在写入属性时调用的函数，默认值为undefined。
```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710175143926.png %}

存取描述符 --是由一对 getter、setter 函数功能来描述的属性

get：一个给属性提供getter的方法，如果没有getter则为undefined。该方法返回值被用作属性值。默认为undefined。

set：一个给属性提供setter的方法，如果没有setter则为undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认值为undefined。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200710174313755.png %}

**注释**

{% img https://yhx0507.github.io/wxapp_static/app/1-Object.definedProperty.png %}

{% img https://yhx0507.github.io/wxapp_static/app/2-Object.definedProperty.png %}

