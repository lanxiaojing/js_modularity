# JS模块化

目的：解决命名冲突，多人协作开发，复用代码，代码结构清晰等。。。



## 原始写法

```function m1() {...}
function m1() {
  ...
}

function m2() {
  ...
}
```

直接调用函数

弊端：造成全局污染，存在依赖关系，比如script标签引入先后顺序等



## 对象写法

```
var m = new Obj({
  m1: function () {
    ...
  },
  m2: function () {
    ...
  }
});
```

调用m对象的属性

弊端：暴露所有模块成员，内部状态可被外部改写



## IIFE

```
var m = (function () {
  ...
})()
```

外部代码无法读取内部变量



## 放大模式

```
var m = (function (mod){
  mod.m3 = function () {
    ...
  }
  
  return mod;
})(m1)
```

若一个模块很大，必须分成几部分，或一个模块需要继承另一个模块



## 宽放大模式

```
var m = (function (mod){
	...
	
	return mod;
})(window.m1 || {})
```

浏览器环境中模块获取加载有先后顺序，放大模式的写法有可能m1未定义





通用的js模块化规范有两种：

## commonJS（nodejs）

每个文件是一个模块，有自己的作用域。其中的变量、函数等对外不可见。

每个模块内部提供一个module （object）变量代表当前模块。

用require加载模块：读入一个js文件，返回这个文件的export对象。



```
var a = require(a);

a.add();
```

服务端的模块化方案，浏览器端不支持。

要调用a.add()必须等a加载完。CommonJS 规范采用同步加载，适用于服务端，但并不适用于浏览器环境（网络延迟、异步特性）。这是AMD诞生的背景。



## AMD(requireJS)

require([module], callback)

```
// 模块封装
define(['./a', './b'], function(a, b) {
  	a.dosomething();
  	// ....
  	b.dosomething();
  	// ....
  	// 返回模块值，可以是函数、对象
  	return function() {}
})

// 引入
require(['./a', './b'], function(a, b) {
	// ...
})
```

依赖前置，提前加载

所有依赖这个模块的语句，都定义在一个回调函数中，等加载完成后，回调函数才执行



## CMD(common module definition; seaJS)

```
// 模块封装
define(function(require, exports, module) {
  var a = require('./a');
  a.dosomething();
  // ....
  var b = require('./b');
  b.dosomething();
  // ....
  // 通过exports／module.exports 提供整个接口
  exports.dosomething = ....
  module.exports = ...
})

// 引入
var c = require('c');
```

依赖就近，延迟加载



## ES6

es6 中import export 方案，目前是通过babel转为commonJS的模式。但也不是完全的commonJS模式。





（未完待续）



