# SLComposeViewController

## 使用步骤
使用SLComposeViewController来实现社交分享的具体步骤如下：
 0. 包含头文件`#import <Social/Social.h>`
 1. 判断设备是否可以向指定的分享平台分享。
 2. 创建分享视图控制器，指定分享平台
 3. 设置分享内容。
 4. 进入分享界面。
 5. 监听用户操作。

## 使用示例
*   判断平台是否可用

```ObjC
if (![SLComposeViewController isAvailableForServiceType:SLServiceTypeSinaWeibo]) {
        NSLog(@"平台不可用,或者没有配置相关的帐号");

        UIAlertView *alert=[[UIAlertView alloc]initWithTitle:@"温馨提示" message:@"您还未绑定新浪微博,请到设置里面绑定" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles: nil];

        [alert show];
        return;
    }
```

*   创建分享视图控制器，并指定分享平台

    
    
    ```ObjC
    // 系统支持的分享功能可以分享的平台有：
     SOCIAL_EXTERN NSString *const SLServiceTypeTwitter
     SOCIAL_EXTERN NSString *const SLServiceTypeFacebook
     SOCIAL_EXTERN NSString *const SLServiceTypeSinaWeibo
     SOCIAL_EXTERN NSString *const SLServiceTypeTencentWeibo
     SOCIAL_EXTERN NSString *const SLServiceTypeLinkedIn
    ```
```ObjC
SLComposeViewController *composeVc = [SLComposeViewController composeViewControllerForServiceType:SLServiceTypeSinaWeibo];
```
*   设置分享内容

```ObjC
// 添加分享的文字
    [composeVc setInitialText:@"这是分享到新浪微博的文字"];

    // 添加图片
    [composeVc addImage:[UIImage imageNamed:@"test.png"]];

    // 添加分享的链接
    [composeVc addURL:[NSURL URLWithString:@"http://www.baidu.com"]];
```
*   弹出分享视图控制器

```ObjC
[self presentViewController:composeVc animated:YES completion:^{
        NSLog(@"进入分享界面");
    }];
```
*   监听用户的操作

```ObjC
composeVc.completionHandler = ^(SLComposeViewControllerResult result) {
        if (result == SLComposeViewControllerResultCancelled) {
            NSLog(@"点击了取消");
        } else if (result == SLComposeViewControllerResultDone) {
            NSLog(@"点击了发送");
        }
    };
```


