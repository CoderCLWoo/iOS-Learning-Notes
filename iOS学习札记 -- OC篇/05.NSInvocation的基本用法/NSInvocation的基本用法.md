# NSInvocation的基本用法
原文链接：*http://www.jianshu.com/p/03e7279a9916*
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。

## 前提：
*   在 iOS中可以直接调用某个对象的消息方式有两种：
一种是`performSelector:withObject`；
再一种就是``NSInvocation``。
第一种方式比较简单，能完成简单的调用。但是对于>2个的参数或者有返回值的处理，那`performSelector:withObject`就显得有点有心无力了，那么在这种情况下，我们就可以使用``NSInvocation``来进行这些相对复杂的操作。

## NSInvocation的基本使用

### 方法签名类
```ObjC
// 方法签名中保存了方法的名称/参数/返回值，协同NSInvocation来进行消息的转发
// 方法签名一般是用来设置参数和获取返回值的, 和方法的调用没有太大的关系
// 1、根据方法来初始化NSMethodSignature
NSMethodSignature  *signature = [ViewController instanceMethodSignatureForSelector:@selector(run:)]; // 或使用： [self methodSignatureForSelector:@selector(run:)];

```
### 根据方法签名来创建NSInvocation对象
```ObjC
// NSInvocation中保存了方法所属的对象/方法名称/参数/返回值
//其实NSInvocation就是将一个方法变成一个对象
// 2、创建NSInvocation对象
NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
// 设置方法调用者
invocation.target = self;
// 注意：这里的方法名一定要与方法签名类中的方法一致
invocation.selector = @selector(run:);
NSString *way = @"byCar";
// 这里的Index要从2开始，以为0跟1已经被占据了，分别是self（target）,selector(_cmd)
[invocation setArgument:&way atIndex:2];
// 3、调用invoke方法
[invocation invoke];
// 实现run:方法
- (void)run:(NSString *)method {

}

```

## 优化
### 上述方法有很多弊端，首先我们来一一解决

*   1、如果调用的方法不存在
    ```ObjC
    // 此时我们应该判断方法是否存在，如果不存在这抛出异常
if (signature == nil) {
// aSelector为传进来的方法
NSString *info = [NSString stringWithFormat:@"%@方法找不到", NSStringFromSelector(aSelector)];
[NSException raise:@"方法调用出现异常" format:info, nil];
    }

    ```
*   2、方法的参数个数与外界传进来的参数数组元素个数不符
    ```ObjC
    // 此处不能通过遍历参数数组来设置参数，因为外界传进来的参数个数是不可控的
// 因此通过 numberOfArguments 方法获取的参数个数,是包含 self 和 _cmd 的，然后比较方法需要的参数和外界传进来的参数个数，并且取它们之间的最小值
NSUInteger argsCount = signature.numberOfArguments - 2;
NSUInteger arrCount = objects.count;
NSUInteger count = MIN(argsCount, arrCount);
for (int i = 0; i < count; i++) {
    id obj = objects[i];
    // 判断需要设置的参数是否是NSNull, 如果是就设置为nil
    if ([obj isKindOfClass:[NSNull class]]) {
        obj = nil;
    }
[invocation setArgument:&obj atIndex:i + 2];
}

    ```
*   3、判断当前调用的方法是否有返回值

```ObjC
// 方法一
id res = nil;
if (signature.methodReturnLength != 0) {//有返回值
    //将返回值赋值给res
    [invocation getReturnValue:&res];
}
return res;

// 方法二：
// 可以通过signature.methodReturnType获得返回的类型编码,因此可以推断返回值的具体类型

```

## 常见应用场景

* OC与js进行交互时，方法的传参调用
