[ios_unity_bridge]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/IOS_UnityBridge.zip
[iOS14 support]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/iOS14_support.md 
[JCSDK]: https://github.com/Romambo/JCSDK  
[DataCollenction_SDK]: https://github.com/Romambo/DataCollection_SDK  
[ADThirdParty_SDK]: https://github.com/Romambo/ADThirdParty_SDK  
[图片1]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/unity_image1.png
[图片2]:https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/ios_image2.png

# JCSDK unity support document

### 中文版本

<details>
<summary>详细文档</summary>
 
- **SDK简介：**  
 JCSDK是MS公司提供的一套广告类型的SDK，内部集成了各大广告商的广告SDK和相关数据统计SDK，便于平台之间对应用内广告的联合运营和数据分析。  
   1. 支持广告类型：  
      开屏广告、banner广告、激励视频广告、插屏广告、native广告  
   2. 版本记录：  
   
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
      
- **SDK接入配置（提供untiy桥接和配置文件）**  
  
  <details>
   <summary>content</summary>
  
    以下是导出Xcode所需的配置，但我们提供了桥接文件和配置文件:[ios_unity_bridge]，来自动集成一些配置，请查看参考使用：  
    ![图片1]
  
   1. SDK库和所需支持库：  
      [JCSDK]  
      [DataCollenction_SDK]  
      [ADThirdParty_SDK]  
   
   2. info.pist 配置：
       ```
       支持http网络配置
       <key>NSAppTransportSecurity</key>
       <dict>
       <key>NSAllowsArbitraryLoads</key>
       <true/>
       </dict>

       Google相关参数配置
       <key>GADApplicationIdentifier</key>
       <string>ca-app-pub-9488501426181082/7319780494</string>
       ```
   3. build setting 配置：  
      bitcode 设置为NO  
      other Linker Flags 设置 -ObjC  
   
   4. iOS14 支持：  
      详情见 [iOS14 support] 说明文档.  
   
   5. 导入系统支持库：  
      Accelerate.framework  
      AdSupport.framework  
      AVFoundation.framework  
      CoreGraphics.framework  
      CoreLocation.framework  
      CoreMedia.framework  
      CoreMotion.framework  
      CoreTelephony.framework  
      iAd.framework  
      MessageUI.framework  
      SafariServices.framework  
      Security.framework  
      SystemConfiguration.framework  
      UIKit.framework  
      VideoToolbox.framework  
      WebKit.framework  
      AppTrackingTransparency.framework  
      libbz2.tbd  
      libc++.tbd  
      libresolv.9.tbd  
      libsqlite3.tbd  
      libxml2.tbd  
      libz.tbd  
   
   6. JCiOSConfig.plist 参数说明：  
      V1.0.0 提供  

      | Item      | Value |
      | --------- | -----:|
      | appid  | JCSDK初始化所需的appid |
      | channelid  | JCSDK初始化所需的channelid |
      | ReYunAppID  | 热云初始化appid |
      | ReYunChannelID  | 热云初始化channleid |   
      | UmengAppID  | Umeng初始化appid |
      | ShuShuAppID  | 数数平台初始化appid |
      | TalkingDataAppID  | TalkingData平台初始化appid |   

      V2.0.0 新增  

      | Item      | Value |
      | --------- | -----:|
      | KochavaAppID  | kochava初始化所需的appid |
      | TenJinAppID  | tenjin初始化所需的appid |
      | ShowSplashFirst  | 首次打开应用是否展示开屏广告，bool类型 YES/NO |
      | LogLevel  | 日志等级：字符串1、关闭。2、开JC日志。3、开JC+ad日志。4、开JC+ad+data 日志 |
   </details>
   
  
- **unity接入Api说明：**  

  <details>
  <summary>content</summary>

  如果文档内API和framework内API有冲突，请以framework内API为准。
   1. 初始化：
      unity开发者，在UnityAppController.mm中实现，应参照下面初始化方式  ：
      先引入头文件：
      ```
      #import <JCSDK/JCSDK>
      #import <AppTrackingTransparency/AppTrackingTransparency.h>
      ```
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
   
   2. 初始化 api：  
      
       ```
       V1.0.0 初始化接口：
       -(void)initJCSDKWithLog:(BOOL)isOpenLog isFirstShowSplash:(BOOL)isShow splashClose:(unityBlock)block;

        V2.0.0 修改初始化接口：
        -(void)initJCSDKWithUnityShow:(unityBlock)block;
       ```
   
   3. banner广告api：  
       ```
        /// isReady - banner
        bool isReadyBanner();

        /// show banner Ads
        void showBannerView();

        /// remove banner Ads
        void removeBannerView();
       ```
   
   4. Intersitial 广告 api：  
       ```
        /// Intersitial Ads isReady
        bool isReadyIntersitial();

        /// show Intersitial Ads
        void showIntersitial();
       ```
   
   5. RewardView广告api：  
       ```
        /// rewardVideo Ads isReady
        bool isReadyRewardVideo();

        /// show rewardVideo Ads
        void showRewardVideo();
       ```
   
  </details>
- **广告接口回调Api和使用：**  
  <details>
  <summary>content</summary>

  1. 接口说明：
        ```
        /// 注册回调监听 ，请在建立广告回传桥接前调用
        void RegistCallBacknotifition();

        /// 用于开屏回调
        /// @param failLoad load失败
        /// @param didShow 展示成功
        /// @param didClick 点击
        /// @param didClose 关闭
        void splash_CallBack(ResultHandler failLoad,ResultHandler didShow, ResultHandler didClick, ResultHandler didClose);

        /// 用于插屏回调
        /// @param failLoad load失败
        /// @param didShow 展示成功
        /// @param failToShow 展示失败
        /// @param didClose 关闭
        /// @param didClick 点击
        /// @param failToPlayVideo 播放video失败
        /// @param startPlayingVideo 开始播放video
        /// @param endPlayingVideo 播放video完成
        void Intersitial_CallBack(ResultHandler failLoad,ResultHandler didShow, ResultHandler failToShow, ResultHandler didClose,ResultHandler didClick,ResultHandler failToPlayVideo, ResultHandler startPlayingVideo, ResultHandler endPlayingVideo);

        /// 用于banner回调
        /// @param failLoad load失败
        /// @param didShow 展示成功
        /// @param didClick 点击
        /// @param didAutoRefresh 自动刷新
        /// @param tapCloseBtn 点击功能关闭按钮
        /// @param failToAutoRefresh 自动刷新失败
        void banner_CallBack(ResultHandler failLoad,ResultHandler didShow,ResultHandler didClick,ResultHandler didAutoRefresh, ResultHandler tapCloseBtn, ResultHandler failToAutoRefresh);

        /// 用于激励视频回调
        /// @param failLoad load失败
        /// @param didRewardSuccess 奖励成功
        /// @param didClose 关闭
        /// @param didClick 点击
        /// @param failToPlayVideo 播放失败
        /// @param startPlayingVideo 开始播放
        /// @param endPlayingVideo 播放完成
        void rewardVideo_CallBack(ResultHandler failLoad,ResultHandler didRewardSuccess, ResultHandler didClose,ResultHandler didClick,ResultHandler failToPlayVideo, ResultHandler startPlayingVideo, ResultHandler endPlayingVideo);

        /// 用于原生广告回调（暂时未开放native广告功能）
        /// @param failLoad load失败
        /// @param didShow 展示成功
        /// @param didClick 点击广告
        /// @param startPlayingVideo 开始播放
        /// @param endPlayingVideo 播放完成
        /// @param tapCloseBtn 点击关闭功能按钮
        /// @param enterFullScreenV 进入全屏video（用于模版）
        /// @param exitFullScreenV exit全屏video（用于模版）
        void native_CallBack(ResultHandler failLoad,ResultHandler didShow, ResultHandler didClick, ResultHandler startPlayingVideo, ResultHandler endPlayingVideo,ResultHandler tapCloseBtn,ResultHandler enterFullScreenV,ResultHandler exitFullScreenV);
        ```
  2. 回调示例:  
        注：回调前先调用注册监听方法，建立连接. 
        插屏回调示例：
    ```
          [DllImport("__Internal")]
  static extern void Intersitial_CallBack(IntPtr failLoad, IntPtr didShow, IntPtr failToShow, IntPtr didClose, IntPtr didClick, IntPtr failToPlayVideo, IntPtr startPlayingVideo, IntPtr endPlayingVideo);

          //注册插屏回调
          var handler11 = new ResultHandler(interFailLoad);
          var fp11 = Marshal.GetFunctionPointerForDelegate(handler11);
          var handler12 = new ResultHandler(interDidShow);
          var fp12 = Marshal.GetFunctionPointerForDelegate(handler12);
          var handler13 = new ResultHandler(interFailtoShow);
          var fp13 = Marshal.GetFunctionPointerForDelegate(handler13);
          var handler14 = new ResultHandler(interDidClose);
          var fp14 = Marshal.GetFunctionPointerForDelegate(handler14);
          var handler15 = new ResultHandler(interDidClick);
          var fp15 = Marshal.GetFunctionPointerForDelegate(handler15);
          var handler16 = new ResultHandler(interFailToPlayVideo);
          var fp16 = Marshal.GetFunctionPointerForDelegate(handler16);
          var handler17 = new ResultHandler(interStartPlayingVideo);
          var fp17 = Marshal.GetFunctionPointerForDelegate(handler17);
          var handler18 = new ResultHandler(interEndPlayingVideo);
          var fp18 = Marshal.GetFunctionPointerForDelegate(handler18);
          Intersitial_CallBack(fp11, fp12, fp13, fp14, fp15, fp16, fp17, fp18);

      //插屏回调
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interEndPlayingVideo(string resultString)
      {
          Debug.Log("插屏回调----->interEndPlayingVideo");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interStartPlayingVideo(string resultString)
      {
          Debug.Log("插屏回调----->interStartPlayingVideo");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interFailToPlayVideo(string resultString)
      {
          Debug.Log("插屏回调----->interFailToPlayVideo");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interDidClick(string resultString)
      {
          Debug.Log("插屏回调----->interDidClick");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interDidClose(string resultString)
      {
          Debug.Log("插屏回调----->interDidClose");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interFailtoShow(string resultString)
      {
          Debug.Log("插屏回调----->interFailtoShow");
      }

      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interDidShow(string resultString)
      {
          Debug.Log("插屏回调----->interDidShow");
      }

      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interFailLoad(string resultString)
      {
          Debug.Log("插屏回调----->interFailLoad");
      }

    ```
    
  </details>

</details>
 
 ----
 
 ### English version
 
<details>
<summary>Detailed documentation</summary>
 
- **SDK Introduction：**  
  JCSDK is a set of advertising SDK provided by MS. It integrates the advertising SDKs of major advertisers and related data statistics SDKs to facilitate the joint operation and data analysis of in-app advertising between platforms.  
   1. support ads：  
      splash ads、banner ads、rewardVideo ads、inter ads、native ads   
   2. Version record：  
   
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
      
- **SDK Access configuration（Provide untiy bridge and configuration files）**  
  
  <details>
   <summary>content</summary>
  
    The following are the configurations required to export Xcode, but we provide bridge files and configuration files to automatically integrate some configurations:[ios_unity_bridge], please check the reference usage:  
    ![图片1]
  
   1. SDK library and required support library：  
      [JCSDK]  
      [DataCollenction_SDK]  
      [ADThirdParty_SDK]  
   
   2. info.pist configuration：
       ```
       Support http network configuration
       <key>NSAppTransportSecurity</key>
       <dict>
       <key>NSAllowsArbitraryLoads</key>
       <true/>
       </dict>

       Google configuration
       <key>GADApplicationIdentifier</key>
       <string>ca-app-pub-9488501426181082/7319780494</string>
       ```
   3. build setting configuration：  
      bitcode  set: NO  
      other Linker Flags set: -ObjC  
   
   4. iOS14 support：  
      see [iOS14 support] document.  
   
   5. Import system support library：  
   
      Accelerate.framework  
      AdSupport.framework  
      AVFoundation.framework  
      CoreGraphics.framework  
      CoreLocation.framework  
      CoreMedia.framework  
      CoreMotion.framework  
      CoreTelephony.framework  
      iAd.framework  
      MessageUI.framework  
      SafariServices.framework  
      Security.framework  
      SystemConfiguration.framework  
      UIKit.framework  
      VideoToolbox.framework  
      WebKit.framework  
      AppTrackingTransparency.framework  
      libbz2.tbd  
      libc++.tbd  
      libresolv.9.tbd  
      libsqlite3.tbd  
      libxml2.tbd  
      libz.tbd  
   
   6. JCiOSConfig.plist Parameter Description：  
        V1.0.0 add  

        | Item      | Value |
        | --------- | -----:|
        | appid  | Appid required for JCSDK initialization |
        | channelid  | ChannelId required for JCSDK initialization |
        | ReYunAppID  | Appid required for reyun initialization |
        | ReYunChannelID  | channelId required for reyun initialization |   
        | UmengAppID  | Appid required for UMeng initialization |
        | ShuShuAppID  | Appid required for 数数 initialization |
        | TalkingDataAppID  | Appid required for TalkingData initialization |   

        V2.0.0 add  

        | Item      | Value |
        | --------- | -----:|
        | KochavaAppID  | Appid required for Kochava initialization |
        | TenJinAppID  | Appid required for tenjin initialization |
        | ShowSplashFirst  | Whether to display an open-screen ad when opening the app for the first time，bool type: YES/NO |
        | LogLevel  | Log level: string type. 1. Close. 2. Open JC log. 3. Open JC+ad log. 4. Open JC+ad+data log |
   </details>
   
  
- **unity Api ：**  

  <details>
  <summary>content</summary>

  If there is a conflict between the API in the document and the API in the framework, please refer to the API in the framework.
   1. init ：
      Unity developers, to implement in UnityAppController.mm, should refer to the following initialization method:  
      First introduce the header file:
      ```
      #import <JCSDK/JCSDK>
      #import <AppTrackingTransparency/AppTrackingTransparency.h>
      ```
      Find the unity entrance: replace startUnity: and call the initialization method of JCSDK. After the SDK is initialized and callback, start "startUnity:"  
        ```
        //[self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
        [self performSelector: @selector(initSDKWithApplication:) withObject: application afterDelay: 0];
        ```
        ```
        -(void)initSDKWithApplication:(UIApplication*)application{
            if (@available(iOS 14, *)) {
                //iOS 14 System IDFA permission box
                [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {

                    //1.0.0 init api
                    //[[JC_unityAdApi getInstance]initJCSDKWithLog:YES isFirstShowSplash:NO splashClose:^(BOOL isOk) {
                    //    [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                    //}];
                    //2.0.0 init api
                    [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                        [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                    }];
                    //to do something，like preloading
                }];
            } else {

                //1.0.0 init api
                //[[JC_unityAdApi getInstance]initJCSDKWithLog:YES isFirstShowSplash:NO splashClose:^(BOOL isOk) {
                //    [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                //}];
                //2.0.0 init api
                [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                    [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                }];
            }
        }
        ```  
   
   2. init api：  
      
       ```
       V1.0.0 init api：
       -(void)initJCSDKWithLog:(BOOL)isOpenLog isFirstShowSplash:(BOOL)isShow splashClose:(unityBlock)block;

        V2.0.0 init api：
        -(void)initJCSDKWithUnityShow:(unityBlock)block;
       ```
   
   3. banner api：  
       ```
        /// isReady - banner
        bool isReadyBanner();

        /// show banner Ads
        void showBannerView();

        /// remove banner Ads
        void removeBannerView();
       ```
   
   4. Intersitial  api：  
       ```
        /// Intersitial Ads isReady
        bool isReadyIntersitial();

        /// show Intersitial Ads
        void showIntersitial();
       ```
   
   5. RewardView api：  
       ```
        /// rewardVideo Ads isReady
        bool isReadyRewardVideo();

        /// show rewardVideo Ads
        void showRewardVideo();
       ```
   
  </details>
- **Advertising interface callback API and use：**  
  <details>
  <summary>content</summary>

  1. Interface Description：
        ```
        /// Sign up for a callback monitor to be invoked before creating a bridge back to the advertiser.
        void RegistCallBacknotifition();

        /// splash callback bridge
        /// @param failLoad 
        /// @param didShow 
        /// @param didClick 
        /// @param didClose 
        void splash_CallBack(ResultHandler failLoad,ResultHandler didShow, ResultHandler didClick, ResultHandler didClose);

        ///  intersitial callback bridge
        /// @param failLoad
        /// @param didShow 
        /// @param failToShow 
        /// @param didClose 
        /// @param didClick 
        /// @param failToPlayVideo 
        /// @param startPlayingVideo 
        /// @param endPlayingVideo
        void Intersitial_CallBack(ResultHandler failLoad,ResultHandler didShow, ResultHandler failToShow, ResultHandler didClose,ResultHandler didClick,ResultHandler failToPlayVideo, ResultHandler startPlayingVideo, ResultHandler endPlayingVideo);

        /// banner callback bridge
        /// @param failLoad load
        /// @param didShow 
        /// @param didClick 
        /// @param didAutoRefresh 
        /// @param tapCloseBtn 
        /// @param failToAutoRefresh 
        void banner_CallBack(ResultHandler failLoad,ResultHandler didShow,ResultHandler didClick,ResultHandler didAutoRefresh, ResultHandler tapCloseBtn, ResultHandler failToAutoRefresh);

        /// rewardVideo  callback bridge
        /// @param failLoad 
        /// @param didRewardSuccess 
        /// @param didClose 
        /// @param didClick 
        /// @param failToPlayVideo 
        /// @param startPlayingVideo 
        /// @param endPlayingVideo 
        void rewardVideo_CallBack(ResultHandler failLoad,ResultHandler didRewardSuccess, ResultHandler didClose,ResultHandler didClick,ResultHandler failToPlayVideo, ResultHandler startPlayingVideo, ResultHandler endPlayingVideo);

        /// native  callback bridge（Not in use yet）
        /// @param failLoad 
        /// @param didShow 
        /// @param didClick 
        /// @param startPlayingVideo 
        /// @param endPlayingVideo 
        /// @param tapCloseBtn 
        /// @param enterFullScreenV 
        /// @param exitFullScreenV
        void native_CallBack(ResultHandler failLoad,ResultHandler didShow, ResultHandler didClick, ResultHandler startPlayingVideo, ResultHandler endPlayingVideo,ResultHandler tapCloseBtn,ResultHandler enterFullScreenV,ResultHandler exitFullScreenV);
        ```
  2. Callback example:  
        Note: Call the registration monitoring method before callback to establish a connection.   
        Interstitial callback example:  
    ```
          [DllImport("__Internal")]
  static extern void Intersitial_CallBack(IntPtr failLoad, IntPtr didShow, IntPtr failToShow, IntPtr didClose, IntPtr didClick, IntPtr failToPlayVideo, IntPtr startPlayingVideo, IntPtr endPlayingVideo);

          // 
          var handler11 = new ResultHandler(interFailLoad);
          var fp11 = Marshal.GetFunctionPointerForDelegate(handler11);
          var handler12 = new ResultHandler(interDidShow);
          var fp12 = Marshal.GetFunctionPointerForDelegate(handler12);
          var handler13 = new ResultHandler(interFailtoShow);
          var fp13 = Marshal.GetFunctionPointerForDelegate(handler13);
          var handler14 = new ResultHandler(interDidClose);
          var fp14 = Marshal.GetFunctionPointerForDelegate(handler14);
          var handler15 = new ResultHandler(interDidClick);
          var fp15 = Marshal.GetFunctionPointerForDelegate(handler15);
          var handler16 = new ResultHandler(interFailToPlayVideo);
          var fp16 = Marshal.GetFunctionPointerForDelegate(handler16);
          var handler17 = new ResultHandler(interStartPlayingVideo);
          var fp17 = Marshal.GetFunctionPointerForDelegate(handler17);
          var handler18 = new ResultHandler(interEndPlayingVideo);
          var fp18 = Marshal.GetFunctionPointerForDelegate(handler18);
          Intersitial_CallBack(fp11, fp12, fp13, fp14, fp15, fp16, fp17, fp18);

      // inter callback
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interEndPlayingVideo(string resultString)
      {
          Debug.Log("inter callback----->interEndPlayingVideo");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interStartPlayingVideo(string resultString)
      {
          Debug.Log("inter callback----->interStartPlayingVideo");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interFailToPlayVideo(string resultString)
      {
          Debug.Log("inter callback----->interFailToPlayVideo");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interDidClick(string resultString)
      {
          Debug.Log("inter callback----->interDidClick");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interDidClose(string resultString)
      {
          Debug.Log("inter callback----->interDidClose");
      }
      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interFailtoShow(string resultString)
      {
          Debug.Log("inter callback----->interFailtoShow");
      }

      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interDidShow(string resultString)
      {
          Debug.Log("inter callback----->interDidShow");
      }

      [MonoPInvokeCallback(typeof(ResultHandler))]
      static void interFailLoad(string resultString)
      {
          Debug.Log("inter callback----->interFailLoad");
      }

    ```
    
  </details>
</details>

