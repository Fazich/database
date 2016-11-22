# 代理模式
## 定义
为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。

代理模式的关键是，当客户不方便直接访问一个对象或者不满足需要的时候，提供一个替身对象来控制对这个对象的访问，客户实际上访问的是替身对象。替身对象对请求做出一些处理之后，再把请求转交给本体对象。

代理模式主要由三部分组成，分别是：

1. 客户类。也就是请求发起方。
2. 代理类。作为客户请求的承接，将请求转发给本体。
3. 本体类。也就是指令的接收方。

## 示例代码
```
/**************** 计算乘积 *****************/
var mult = function() {
    var a = 1;
    for (var i = 0, l = arguments.length; i < l; i++) {
        a = a * arguments[i];
    }
    return a;
};

/**************** 计算加和 *****************/
var plus = function() {
    var a = 0;
    for (var i = 0, l = arguments.length; i < l; i++) {
        a = a + arguments[i];
    }
    return a;
};

/**************** 创建缓存代理的工厂 *****************/
var createProxyFactory = function(fn) {
    var cache = {}; //缓存队列
    return function() {
        var args = Array.prototype.join.call(arguments, ',');
        if (args in cache) {
            return cache[args]; //如果请求曾经计算过直接吐出结果
        }
        return cache[args] = fn.apply(this, arguments);
    }

};

var proxyMult = createProxyFactory(mult),
    proxyPlus = createProxyFactory(plus);

alert(proxyMult(1, 2, 3, 4)); //计算输出24
alert(proxyMult(1, 2, 3, 4)); //缓存输出24
alert(proxyPlus(1, 2, 3, 4)); //计算输出10
alert(proxyPlus(1, 2, 3, 4)); //缓存输出10
```
