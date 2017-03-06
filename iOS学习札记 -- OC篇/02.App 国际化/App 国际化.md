# App 国际化

### 原理

*   **从代码中分离文本：**
目前，应用展示的所有文本都是以硬编码字符串存在于`Main.storyboard` 和 ViewController 等文件 里。为了本地化这些字符串，你需要把它们放在一个单独的文件中。他将会在包中简单地引用这些字符串，而不是在你的方法中进行硬编码。
`Xcode` 使用带有` .strings` 扩展名的文件来存储和检索app中使用的所有字符串，以支持每种语言。根据 iOS 设备当前使用的语言，代码中一个简单的方法调用将会查找并返回要求的字符串。

### 获取系统所支持的国际化信息

*   在国际化之前,你可以在iphone中的`设置->通用->语言与地区->iPhone语言`中来查看你的iphone支持哪些语言,当然也可以写一段代码测试一下你的iphone都支持哪些语言.测试代码如下:
    ```ObjC
    // 注:NSUserDefaults类用来取得用户的默认信息.
    NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
    NSArray *languages = [defaults objectForKey:@"AppleLanguages"];
    NSLog(@"%@", languages);

    ```

### 在`Xcode中`建立多语言文档
#### 内容国际化
*   1.`右鍵 -> New File…`, 在`Resources`分类下选择`Strings File`新建文档;
*   2.将文件保存名设置为`Localizable.strings` (`Localizable.strings` 是系统默认多语言文件名);
*   单击 `Localizable.strings` 文件，查看右边的属性，在 `Localization` 栏添加语言。
*   添加需要国际化的`语言内容`:
    在 `Localizable.strings` 中，按照`"key" ＝ "value";` 的格式, 然后使用时用`NSLocalizedString(@"key", @"")`读取内容. 如果不是用系统默认名字, 那么使用 `NSLocalizedStringFromTable(<#key#>, <#tbl#>, <#comment#>)`方法，在 `tbl` 参数填上你的语言文件名。例如：``NSLocalizedStringFromTable(@"key", @"LocalString", @"");``(此处语言文件名为LocalString.strings)。
    *   在 `Localization.strings English` 文件添加 `"key" ＝ "hello world";`
    *   在 `Localization.strings Chinese` 文件添加 `"key" ＝ "世界 你好";`

```ObjC
注意：使用 NSLocalizedString 的时候，文件名必须是 Localizable.strings
```



#### 程序名称国际化
*   同样的方法，新增一个文件名为 `InfoPlist.strings` 的语言文件
*   设置程序名称
    *   在 `English`的语言文件添加：`CFBundleDisplayName = "hello world";`
    *   在 `Chinese` 的语言文件添加：`CFBundleDisplayName = "世界 你好";`
