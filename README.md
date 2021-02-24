
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

<details>
<summary>3.0.0</summary>

支持开发者工具: Xcode 12  
系统版本:iOS 9.0  

**更新内容**  
>1.更新SDK版本3.0.0，支持topon5.7.13 以及对应的第三方广告库  
>2.优化内部开屏相关逻辑和启动广告请求延时加载功能  

</details>

## Version of the record

<details>
<summary>1.0.0</summary>

support development tools: Xcode 11  
system version:iOS 9.0
</details>
 
<details>
<summary>2.0.0</summary>

support development tools: Xcode 12  
system version:iOS 9.0

**update content**  
>1.Added internal logic waterfall and continuous display  
>2.Added "kochava" and "tenjin" statistics  
>3.Change the SDK initialization interface used by Unity. see: JC_unityAdApi.h
```
old code
//-(void)initJCSDKWithLog:(BOOL)isOpenLog isFirstShowSplash:(BOOL)isShow splashClose:(unityBlock)block;
new code
-(void)initJCSDKWithUnityShow:(unityBlock)block;
```

>4.Change the log log interface, increase the log level.  see: JCAdCallBackHeader.h  
```
old code
//+(void)setOpenPlatformLog:(BOOL)openPlatformLog;
new code
+(void)setTheLogLevel:(MSLogLevelStatus)logLevel;
```

>5.Change JCiOSConfig.plist, add:   
   "KochavaAppID":    kochava initialization parameters   
   "TenJinAppID":     TenJin initialization parameters   
   "ShowSplashFirst": Whether to display splash when the app is first opened. 
   "LogLevel":loglevel 1、closeAll. 2、open JC_log. 3、open JC+AD log. 4、open JC+AD+Data log. Defaults:1  

**Project configuration：**  
* add System library:  
   > AppTrackingTransparency.framework  
* add Third party library and file:
   > KochavaCore.framework               (Embed & Sign)  
   > KochavaTracker.framework            (Embed & Sign)  
   > KochavaAdNetwork.framework          (Embed & Sign)  
   > libTenjinSDK.a  
   > TenjinSDK.h 
</details>

<details>
<summary>3.0.0</summary>

support development tools: Xcode 12  
system version:iOS 9.0  

**update content**   
>1.Update SDK version 3.0.0, support topo 5.7.13 and corresponding third-party advertising library  
>2.Optimize internal screen-opening related logic and start the delayed loading function of ad request  

</details>
