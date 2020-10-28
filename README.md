
## If the access interface has changed, please see the version changes
## 如果接入接口有变动，请参阅版本变化

## 版本记录
<details>
<summary>1.0.0</summary>

支持开发者工具: Xcode 11
系统版本:iOS 9.0
</details>
 
<details>
<summary>2.0.0</summary>

支持开发者工具: Xcode 12
系统版本:iOS 9.0

**更新内容**  
>1.新增了流量组和连续展示功能逻辑、升级内部广告接口 V4 -> V5  
>2.新增 "kochava" and "tenjin" 数据统计平台  
>3.修改了unity使用者需要接入的OC初始化接口. 详情见: JC_unityAdApi.h
```
旧代码
//-(void)initJCSDKWithLog:(BOOL)isOpenLog isFirstShowSplash:(BOOL)isShow splashClose:(unityBlock)block;
新代码
-(void)initJCSDKWithUnityShow:(unityBlock)block;
```

>4.修改了iOS日志打印接口。新增日志等级功能，详情见: JCAdCallBackHeader.h  
```
旧代码
//+(void)setOpenPlatformLog:(BOOL)openPlatformLog;
新代码
+(void)setTheLogLevel:(MSLogLevelStatus)logLevel;

```

>5.修改了JCiOSConfig.plist文件, 新增字段:   
   "KochavaAppID":    kochava 初始化参数   
   "TenJinAppID":     TenJin 初始化参数   
   "ShowSplashFirst": 应用首次打开是否展示开屏广告. 
   "LogLevel":日志等级 1、关闭. 2、打开JC日志. 3、打开JC+广告日志. 4、打开JC+广告+数据日志. 默认值:1  

**项目配置：**  
* 添加系统库:  
   > AppTrackingTransparency.framework  
* 添加第三方库和文件:
   > KochavaCore.framework               (Embed & Sign)  
   > KochavaTracker.framework            (Embed & Sign)  
   > KochavaAdNetwork.framework          (Embed & Sign)  
   > libTenjinSDK.a  
   > TenjinSDK.h 
</details>


## Version of the record

<details>
<summary>1.0.0</summary>

support development tools: Xcode 11
</details>
 
<details>
<summary>2.0.0</summary>

support development tools: Xcode 12  
**update content**  
>1.Added internal logic waterfall and continuous display  
>2.Added "kochava" and "tenjin" statistics  
>3.Change the SDK initialization interface used by Unity. see: JC_unityAdApi.h  
>4.Change the log log interface, increase the log level.  see: JCAdCallBackHeader.h  
>5.Change JCiOSConfig.plist, add: "KochavaAppID", "TenJinAppID", "ShowSplashFirst", "LogLevel"  

**Project configuration：**  
* add Support Library and file:  
   > AppTrackingTransparency.framework  
   > KochavaCore.framework               (Embed & Sign)  
   > KochavaTracker.framework            (Embed & Sign)  
   > KochavaAdNetwork.framework          (Embed & Sign)  
   > libTenjinSDK.a  
   > TenjinSDK.h 
</details>

