#  单例模式
## 定义
保证一个类仅有一个实例，并提供一个访问它的全局访问点。

也就是说一个类只能有一个接口，且只能通过这个接口来访问他，通过这种方式可以节约内存，实现类中实例的最大复用

## 代码示例
```
var ProxySingletonCreateDiv = (function(){
  var instance; // 实例保存对象
  return function(html){
    if ( !instance ){
      instance = new CreateDiv(html); ／／内部声明
    }
    return instance; ／／再次调用时返回该实例
  }
})();
  var a = new ProxySingletonCreateDiv('sven1');
  var b = new ProxySingletonCreateDiv('sven2');
  alert ( a === b ); ／／true
```
