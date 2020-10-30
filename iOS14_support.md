[App Tracking Transparency]: https://developer.apple.com/documentation/apptrackingtransparency?language=objc  
[requestTrackingAuthorizationWithCompletionHandler:]: https://developer.apple.com/documentation/apptrackingtransparency/attrackingmanager/3547037-requesttrackingauthorization  
[SKAdNetwork]: https://developer.apple.com/documentation/storekit/skadnetwork  
[Google]: https://developers.google.com/admob/ios/ios14  
[穿山甲(Pangle)]: https://ad.oceanengine.com/union/media/union/download  
[IronSource]: https://developers.ironsrc.com/ironsource-mobile/ios/ironsource-sdk7-update-guide/  
[UnityAds]: https://unityads.unity3d.com/help/ios/integration-guide-ios  
[AdColony]: https://github.com/AdColony/AdColony-iOS-SDK/wiki/Project-Setup#step-4-configuring-ad-networks  
[Mintegral]: http://cdn-adn.rayjump.com/cdn-adn/v2/markdown_v2/index.html?file=sdk-m_sdk-ios&lang=cn  
[Sigmob]: http://docs.sigmob.cn/#/sdk/SDK%E6%8E%A5%E5%85%A5/ios/?id=ios14%e7%9b%b8%e5%85%b3%e6%94%af%e6%8c%81  
[Maio]: https://github.com/imobile-maio/maio-iOS-SDK  
[Vungle]: https://support.vungle.com/hc/zh-cn/articles/360002925791  


# iOS 14 Support

## 中文版本

<details>
<summary>中文版本</summary>

### 概述  
从iOS 14开始，只有在获得用户明确许可的前提下，应用才可以访问用户的IDFA数据并向用户投放定向广告。在应用程序调用[App Tracking Transparency]框架向最终用户提出应用程序跟踪授权请求之前，IDFA将不可用。如果某个应用未提出此请求，则读取到的IDFA将返回全为0的字符串。本指南将介绍iOS 14支持所需的更改。

### 如何支持iOS 14 
<details>
**<summary><font color=green>使用用户权限获取IDFA</font></summary>
 
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
        //iOS 14 系统IDFA权限弹框
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            
            //1.0.0初始化接口
            //[[JC_unityAdApi getInstance]initJCSDKWithLog:YES isFirstShowSplash:NO splashClose:^(BOOL isOk) {
            //    [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
            //}];
            //2.0.0初始化接口
            [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
            }];
            //to do something，like preloading
        }];
    } else {
        
        //1.0.0初始化接口
        //[[JC_unityAdApi getInstance]initJCSDKWithLog:YES isFirstShowSplash:NO splashClose:^(BOOL isOk) {
        //    [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
        //}];
        //2.0.0初始化接口
        [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
            [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
        }];
    }
}
```
</details>
<details>
<summary>**使用SKAdNetwork跟踪转化：**</summary>
   
使用Apple的转化跟踪SKAdNetwork，这意味着即使IDFA不可用，广告平台也可以通过这个获取应用安装归因。请参阅Apple的[SKAdNetwork]文档，以了解更多信息。  
> 要启用此功能，您需要在info.plist中添加SKAdNetworkItems。目前JCSDK版本兼容的三方广告平台中，支持iOS 14的平台如下。开发者根据集成的情况，可分别添加对应平台的SKAdNetwork标识符,现在支持的平台有：Google Admob、穿山甲（Pangle）、IronSource、UnityAds、ADColony、Mintegral、Sigmob、Maio、Vungle  
<details>
<summary>Google Admob</summary>

请参阅 [Google] 文档，以了解更多信息  
```
在info.plist中添加SKAdNetworkItems
<key>SKAdNetworkItems</key>
<array>
    <dict>
      <key>SKAdNetworkIdentifier</key>
      <string>cstr6suwn9.skadnetwork</string>
    </dict>
</array>
```
</details>

<details>
<summary>穿山甲（Pangle）</summary>

请参阅 [穿山甲(Pangle)] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key>
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>238da6jt44.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>22mmun2rn5.skadnetwork</string>
    </dict>
</array>
```  
</details>

<details>
<summary>IronSource</summary>

请参阅 [IronSource] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key>
<array>   
    <dict>       
        <key>SKAdNetworkIdentifier</key>      
        <string>SU67R6K2V3.skadnetwork</string>   
    </dict>
</array>
```
</details>

<details>
<summary>UnityAds</summary>

请参阅 [UnityAds] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key>
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>4DZT52R2T5.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>bvpn9ufa9b.skadnetwork</string>
    </dict>
</array>
```
</details>

<details>
<summary>AdColony</summary>

请参阅 [AdColony] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key><array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>4PFYVQ9L8R.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>YCLNXRL5PM.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>V72QYCH5UU.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>TL55SBB4FM.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>T38B2KH725.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>PRCB7NJMU6.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>PPXM28T8AP.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>MLMMFZH3R3.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>KLF5C3L5U5.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>HS6BDUKANM.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>C6K4G5QG8M.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>9T245VHMPL.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>9RD848Q2BZ.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>8S468MFL3Y.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>7UG5ZH24HU.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>4FZDC2EVR5.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>4468KM3ULZ.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>3RD42EKR43.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>2U9PT9HC89.skadnetwork</string>
    </dict></array>
```
</details>

<details>
<summary>Mintegral</summary>

请参阅 [Mintegral] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key><array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>KBD757YWX3.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>wg4vff78zm.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>737z793b9f.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>ydx93a7ass.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>prcb7njmu6.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>7UG5ZH24HU.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>44jx6755aq.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>2U9PT9HC89.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>W9Q455WK68.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>YCLNXRL5PM.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>TL55SBB4FM.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>8s468mfl3y.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>GLQZH8VGBY.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>c6k4g5qg8m.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>mlmmfzh3r3.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>4PFYVQ9L8R.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>av6w8kgt66.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>6xzpu9s2p8.skadnetwork</string>
    </dict>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>hs6bdukanm.skadnetwork</string>
    </dict></array>
```
</details>

<details>
<summary>Sigmob</summary>

请参阅 [Sigmob] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key>
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>58922NB4GD.skadnetwork</string>
    </dict>
</array>
```
</details>

<details>
<summary>Maio</summary>

请参阅 [Maio] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key>
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>V4NXQHLYQP.skadnetwork</string>
    </dict>
</array> 
```
</details>
<details>
<summary>Vungle</summary>

请参阅 [Vungle] 文档，以了解更多信息  
```
<key>SKAdNetworkItems</key>
<array>
    <dict>
        <key>SKAdNetworkIdentifier</key>
        <string>GTA9LK7P23.skadnetwork</string>
    </dict>
</array>
```
</details>

 </details>

</details>
 

