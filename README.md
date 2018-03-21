# LightJSFix

# Overview
一个轻量级的JavaScript-ObjectiveC 通信框架。
提供JS 与 ObjectiveC之间的互相调用，支持大部分的基本类型，OC对象 以及 OC里的大部分结构(如CGSize，NSRange，CGRect等)类型参数。
支持从 OC 传Block 到JS里，反向暂不支持。

# Implementation
在一个独立的 JSContext，注册了几个主要方法供JS调用OC方法。
runBeforeInstanceMethod(String instanceClassName, String methodName, function(instance, originalInvocation, arguments) {
...
})
此方法是在指定的类（instanceClassName）的某个实例方法（methodName）执行前，插入另一个执行函数（即第三个参数）。
该执行函数固定三个参数。
参数1（instance）：执行此实例方法的 对象。
参数2（originalInvocation）：该实例方法的NSInvocation（给 runInvocation 方法做参数，来调用原函数）
参数3（argments）：该实例方法的所有参数 数组。
下面几个方法同理：
replaceInstanceMethod：用自定义的 function 替换原实例方法。
runAfterInstanceMethod：在此实例方法执行后，插入执行另一个指定函数。

runBeforeClassMethod：在指定的类方法执行前，插入执行一个函数。
replaceClassMethod：用自定义的function 替换原类方法。
runAfterClassMethod：在此类方法执行后，插入执行另一个指定函数。

上面几个方法提供了实例方法和类方法的替换功能，因此可以用来做hotfix。



# Usage

