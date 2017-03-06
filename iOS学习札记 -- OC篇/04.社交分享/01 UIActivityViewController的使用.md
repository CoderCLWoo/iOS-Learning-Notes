# UIActivityViewController

参考文章：
*   [UIActivityViewController系统原生分享-仿简书分享](http://www.jianshu.com/p/b6b44662dfda)
*   [iOS开发 - UIActivityViewController详解](http://www.tuicool.com/articles/JRVRjy6)

## 使用步骤
使用`UIActivityViewController`我们可以选中需要分享的平台，然后跳转到分享内容的边界界面，具体的实现步骤如下：
   1. 设置分享内容。
   2. 创建分享列表的控制器，并传入分享内容
   3. 推出分享视图控制器。

## 初始化Activity View Controller
```ObjC
- (instancetype)initWithActivityItems:(NSArray *)activityItems applicationActivities:(nullable NSArray<__kindof UIActivity *> *)applicationActivities;
```
#### 参数说明
*   activityItems
：在执行activity中用到的数据对象数组。数组中的对象类型是可变的，并依赖于应用程序管理的数据。例如，数据可能是由一个或者多个字符串/图像对象，代表了当前选中的内容。

*   applicationActivities：是一个UIActivity对象的数组，代表了应用程序支持的自定义服务。这个参数可以是nil。

*   返回值
：返回一个将要展现的activity view controller。


## 使用示例

```ObjC
    NSString *textToShare = @"要分享的文本内容";
    UIImage *imageToShare = [UIImage imageNamed:@"iosshare.jpg"];
    NSURL *urlToShare = [NSURL URLWithString:@"http://blog.csdn.net/hitwhylz"];
    NSArray *activityItems = @[textToShare, imageToShare, urlToShare];

    UIActivityViewController *activityVC = [[UIActivityViewController alloc]initWithActivityItems:activityItems applicationActivities:nil];

    // 监听选中分享类型(iOS8以后用completionWithItemsHandler了)
    [activityVC setCompletionWithItemsHandler:^(NSString * __nullable activityType, BOOL completed, NSArray * __nullable returnedItems, NSError * __nullable activityError){

        // 显示选中的分享类型
        NSLog(@"act type %@",activityType);

        if (completed) {
            NSLog(@"ok");
        }else {
            NSLog(@"no ok");
        }

    }];
```

*   excludedActivityTypes属性

```ObjC
@property(nullable, nonatomic, copy) NSArray<UIActivityType> *excludedActivityTypes; // default is nil. activity types listed will not be displayed
```
默认情况下，UIActivityViewController 将显示所有可用于所提供内容的服务，但我们也可以排除特定的 Activity 类型。
这就要利用excludedActivityTypes属性了，它可以`声明我们不要显示出来的服务列表`。
Activity 类型又分为`“操作”`和`“分享”`两大类, 具体看名称就能区分了。

```ObjC
    UIKIT_EXTERN NSString *const UIActivityTypePostToFacebook     NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypePostToTwitter      NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypePostToWeibo        NS_AVAILABLE_IOS(6_0);    // SinaWeibo
    UIKIT_EXTERN NSString *const UIActivityTypeMessage            NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypeMail               NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypePrint              NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypeCopyToPasteboard   NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypeAssignToContact    NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypeSaveToCameraRoll   NS_AVAILABLE_IOS(6_0);
    UIKIT_EXTERN NSString *const UIActivityTypeAddToReadingList   NS_AVAILABLE_IOS(7_0);
    UIKIT_EXTERN NSString *const UIActivityTypePostToFlickr       NS_AVAILABLE_IOS(7_0);
    UIKIT_EXTERN NSString *const UIActivityTypePostToVimeo        NS_AVAILABLE_IOS(7_0);
    UIKIT_EXTERN NSString *const UIActivityTypePostToTencentWeibo NS_AVAILABLE_IOS(7_0);
    UIKIT_EXTERN NSString *const UIActivityTypeAirDrop            NS_AVAILABLE_IOS(7_0);
    UIKIT_EXTERN UIActivityType const UIActivityTypeOpenInIBooks       NS_AVAILABLE_IOS(9_0)
```

这里，我们不需要显示的类型为：

```ObjC
activityVC.excludedActivityTypes = @[UIActivityTypeAssignToContact,
                                           UIActivityTypePrint];

```

*   展示

    在展现view controller时，必须根据当前的设备类型，使用适当的方法。在iPad上，必须通过popover来展现view controller。在iPhone和iPodtouch上，必须以模态的方式展现。

```ObjC
//以模态的方式展现activityVC。
[self presentViewController:activityVC animated:YES completion:nil];
```



