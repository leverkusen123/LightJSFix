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

替换 & 前后插入function 主要使用了强大的开源类 Aspects；它能在替换指定method的 IMP后，保留替换前的 IMP 并以NSInvocation方式存在。

上面几个方法提供了实例方法和类方法的替换功能，因此可以用来做hotfix。


但我们还需要提供JS调用OC方法的功能，最简单的方法是 包装一个支持变参的performSelector给到JS。
主要有下面5个方法：
runInstanceMethodWithReturn：调用一个有返回值的实例方法
runInstanceMethodWithNoReturn：调用一个没有返回值的实例方法
以上两个个方法，前两个参数都是固定的，第一个是实例对象，第二个是方法名；后面跟的是这个方法的参数，数量必须跟方法定义的一致。

runClassMethodWithReturn：调用一个有返回值的类方法
runClassMethodWithNoReturn：调用一个没有返回值的类方法
以上两个个方法，前两个参数都是固定的，第一个是类名，第二个是方法名；后面跟的是这个类方法的参数，数量必须跟方法定义的一致。
例如：
var dict = runClassMethodWithReturn('NSMutableDictionary', 'dictionary')
runInstanceMethodWithNoReturn(dict, 'setObject:forKey:', 'value', 'key')
var value = runInstanceMethodWithReturn(dict, 'objectForKey:', 'key');



# Usage

