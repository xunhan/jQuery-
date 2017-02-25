## Jqeury 2.0.3 精读笔记

```
    dom 文档的加载顺序
    1. 解析 HTML 结构
    2. 加载外部脚本和样式表文件
    3. 解析并执行脚本代码
    4. 构造 HTML、DOM 模型。 // ready
    5. 加载图片等外部文件
    6. 页面记载完毕 // load
```

### 第一部分 0~281 行

+ 自执行函数
```
(function(window,undefined){}))(window)
```
1. window 属于全局对象，函数内部查找较慢，通过把 window 作为参数传入，window 对象变为匿名函数的局部变量，函数内部访问变快
2. 当代码压缩时进行优化
3. undefined 在 js 中不是保留字，由于没有传递第二个参数，这样可以避免 undefined 作为变量被改变（这是为了兼容低版本浏览器，在 ie8 及以下是可以被改变的）

+ 严格模式
```
'use strict'
```
1. 消除 JS 语句中一些不合理、不严谨之处，减少一些怪异行为；
2. 消除代码运行的一些不安全指出，保证代码运行的安全；
3. 提高编译器效率，增加运行速度；
4. 为未来新版本 JS 做好铺垫
> 缺点：  
一些低版本浏览器不支持，甚至会出现假死状态

+ 定义变量

1. 给予赋值内容含义，便于理解
2. 全局变量存储，减少获取次数与查询成本，提高执行效率
3. 便于压缩

+ 原型上添加方法的不同
```
    function A(){};
    1. A.prototype.myName= 'A',A.prototype.myAge='0';
    // 这相当于在原型上添加方法
    var newa = new A();
    // newa.constructor 指向  A.prototype.constructor
    而 A.prototype={},这种方法相当于重新给 A.prototype 赋值，重写过后
    newa.constructor 指向 Object 也就是 {} 的 constructor;
    2.所以，在使用 对象字面量形式的时候应该重新修正：
        A.prototype={
            'constructor':A
        }
```

+ 正则

```
    rquickExpr = /^(?:\s*(<[\w\W]+>)[^>]*|#([\w-]*))$/;
    
    1. 正则前瞻（?=）会作为匹配较检，但不会出现在匹配结果字符串里面
    2. 非捕获性分组(?:) 回座位匹配较检，并出现在匹配结果字符串里面，如果出现在(...)，不作为子匹配返回
    var str = 'u r so handsome';
    /so\s*(?=\w+)/.exec(str) //['so ']
    /so\s*(?:\w+)/.exec(str) //['so handsome']
    /so\s*(\w+)/.exec(str) //['so handsome','handsome']
```
> RegExpObject.exec 方法返回一个数组，其中包含 index(匹配起始索引) input(原始整个字符串)，如未找到匹配返回 null；

```
    rsingleTag = /^<(\w+)\s*\/?>(?:<\/\1>|)$/
    1. \1 捕获组，匹配第一个小括号内的值
```

+ 压栈
    - 后进先出，实现 end();

> 构造函数上的方法称为 **静态方法**（工具方法），原型对象上的方法称为 **实例方法**。
