#  策略模式
##  定义
定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。

策略模式的核心在于由三大部分组成，它们分别是：

1. 策略对象。用来封装一系列的算法实现的。
2. 抽象策略角色。作为客户端与策略对象之间相互调用的桥梁。
3. 环境角色。也就是客户端最终调用的部分

## 应用场景
1. 多个类只区别在表现行为不同，可以使用Strategy模式，在运行时动态选择具体要执行的行为。
2. 需要在不同情况下使用不同的策略(算法)，或者策略还可能在未来用其它方式来实现。
3. 对客户隐藏具体策略(算法)的实现细节，彼此完全独立。

## 实例代码
在表单验证这个环节中非常适合使用策略模式

策略对象：
```
var strategies = {
  isNonEmpty: function(value, errorMsg){
    if (value === ''){
      return errorMsg;
    }
  },
  minLength: function(value,length,errorMsg){
    if(value.length < length){
      return errorMsg;
    }
  },
  isMobile: function(value, errorMsg){
    if (!/(^1[3|5|8][0-9]{9}$)/.test(value)){
      return errorMsg;
    }
  }
};
```
抽象策略角色，用来连接客户端与策略对象
```
var Validator = function() {
    this.cache = [];／／类似一个发布订阅， 将需要的验证的dom存入到这个缓存队列里
};

Validator.prototype.add = function(dom, rules) {
    var self = this;
    for (var i = 0, rule; rule = rules[i++];) {
        (function(rule) {
            var strategyAry = rule.strategy.split(':');
            var errorMsg = rule.errorMsg;
            self.cache.push(function() {
                var strategy = strategyAry.shift();
                strategyAry.unshift(dom.value);
                strategyAry.push(errorMsg);
                return strategies[strategy].apply(dom, strategyAry);
            });
        })(rule)
    }
};

Validator.prototype.start = function() {
    for (var i = 0, validatorFunc; validatorFunc = this.cache[i++];) {
        var errorMsg = validatorFunc();
        if (errorMsg) {
            return errorMsg;
        }
    }
};
```

客户端调用去代码
```
var registerForm = document.getElementById('registerForm');

var validataFunc = function() {
    var validator = new Validator();
    validator.add(registerForm.userName, [{
        strategy: 'isNonEmpty',
        errorMsg: '用户名不能为空'
    }, {
        strategy: 'minLength:6',
        errorMsg: '用户名长度不能小于 10 位'
    }]);
    validator.add(registerForm.password, [{
        strategy: 'minLength:6',
        errorMsg: '密码长度不能小于 6 位'
    }]);
    validator.add(registerForm.phoneNumber, [{
        strategy: 'isMobile',
        errorMsg: '手机号码格式不正确'
    }]);
    var errorMsg = validator.start();
    return errorMsg;
}

registerForm.onsubmit = function() {
    var errorMsg = validataFunc();
    if (errorMsg) {
        alert(errorMsg);
        return false;
    }
}

```
