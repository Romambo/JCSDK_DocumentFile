[App Tracking Transparency]: https://developer.apple.com/documentation/apptrackingtransparency?language=objc  
[requestTrackingAuthorizationWithCompletionHandler:]: https://developer.apple.com/documentation/apptrackingtransparency/attrackingmanager/3547037-requesttrackingauthorization  
[SKAdNetwork文档]: https://developer.apple.com/documentation/storekit/skadnetwork  
[Google 文档]: https://developers.google.com/admob/ios/ios14  
[穿山甲(Pangle) 文档]: https://ad.oceanengine.com/union/media/union/download  
[IronSource 文档]: https://developers.ironsrc.com/ironsource-mobile/ios/ironsource-sdk7-update-guide/  
[UnityAds 文档]: https://unityads.unity3d.com/help/ios/integration-guide-ios  
[AdColony 文档]: https://github.com/AdColony/AdColony-iOS-SDK/wiki/Project-Setup#step-4-configuring-ad-networks  
[Mintegral 文档]: http://cdn-adn.rayjump.com/cdn-adn/v2/markdown_v2/index.html?file=sdk-m_sdk-ios&lang=cn  
[Sigmob 文档]: http://docs.sigmob.cn/#/sdk/SDK%E6%8E%A5%E5%85%A5/ios/?id=ios14%e7%9b%b8%e5%85%b3%e6%94%af%e6%8c%81  
[Maio 文档]: https://github.com/imobile-maio/maio-iOS-SDK  
[Vungle 文档]: https://support.vungle.com/hc/zh-cn/articles/360002925791  


# iOS 14 Support

## 中文版本

<details>
<summary>中文版本</summary>

### 概述  
从iOS 14开始，只有在获得用户明确许可的前提下，应用才可以访问用户的IDFA数据并向用户投放定向广告。在应用程序调用[App Tracking Transparency]框架向最终用户提出应用程序跟踪授权请求之前，IDFA将不可用。如果某个应用未提出此请求，则读取到的IDFA将返回全为0的字符串。本指南将介绍iOS 14支持所需的更改。

### 如何支持iOS 14  
* 使用用户权限获取IDFA  
> 添加系统支持库：  
AppTrackingTransparency.framework  
> 在info,plist文件里添加获取IDFA权限描述：  
```
<key>NSUserTrackingUsageDescription</key> 
<string>This identifier will be used to deliver personalized ads to you.</string>
```
![image1](https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/ios14_image1.png)  
> 获取App Tracking Transparency权限：  
想要获取授权，需要使[requestTrackingAuthorizationWithCompletionHandler:],我们建议您在初始化JCSDK之前获取授权，以便如果用户授予允许跟踪权限，JCSDK则可以在广告请求中使用IDFA.  
```
引入头文件
#import <AppTrackingTransparency/AppTrackingTransparency.h>
```
如果你是iOS开发者，应参照下面初始化方式
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    self.window = [[UIWindow alloc]initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    self.window.rootViewController = [[ViewController alloc]init];
    [self.window makeKeyAndVisible];
    if (@available(iOS 14, *)) {
        //iOS 14
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
           // 初始化/init JCSDK
            [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                
            }];
        }];
    } else {
        // 初始化/init JCSDK
        [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
            
        }];
    }
```
如果你是unity开发者，在UnityAppController.mm中实现，应参照下面初始化方式：
找到unity入口 ：替换掉startUnity: 并调用JCSDK的初始化方法，待sdk初始化回调后，再启动startUnity:
```
//[self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
[self performSelector: @selector(initSDKWithApplication:) withObject: application afterDelay: 0];
```
```
-(void)initSDKWithApplication:(UIApplication*)application{
    if (@available(iOS 14, *)) {
        //iOS 14
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            
            //1.0.0初始化接口
            [[JC_unityAdApi getInstance]initJCSDKWithLog:YES isFirstShowSplash:NO splashClose:^(BOOL isOk) {
                [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
            }];
            //2.0.0初始化接口
            [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
            }];
            //to do something，like preloading
        }];
    } else {
        
        //1.0.0初始化接口
        [[JC_unityAdApi getInstance]initJCSDKWithLog:YES isFirstShowSplash:NO splashClose:^(BOOL isOk) {
            [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
        }];
        //2.0.0初始化接口
        [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
            [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
        }];
    }
}
```
</details>
 

