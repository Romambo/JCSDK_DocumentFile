  
[iOS14 support]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/iOS14_support.md 
[JCSDK]: https://github.com/Romambo/JCSDK  
[DataCollenction_SDK]: https://github.com/Romambo/DataCollection_SDK  
[ADThirdParty_SDK]: https://github.com/Romambo/ADThirdParty_SDK  
[图片1]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/ios_image1.png
[图片2]:https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/ios_image2.png

# JCSDK ios support document

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
 
- **SDK接入配置:**  

   <details>
   <summary>content</summary>

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
       <key>GADIsAdManagerApp</key>
       <true/>
       
       获取地理位置权限
       <key>NSLocationWhenInUseUsageDescription</key>
       <string>The app needs to get your location</string>
       
       获取IDFA权限，iOS14支持
       <key>NSUserTrackingUsageDescription</key> 
       <string>This identifier will be used to deliver personalized ads to you.</string>
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
   
- **SDK相关Api:**
   <details>
   <summary>content</summary>

   如果文档内API和framework内API有冲突，请以framework内API为准。
   1. 头文件：
      #import <JCSDK/JCSDK.h>  
   
   2. 初始化SDK：  
       ```
       //appid 和 channelid如果在JCiOSConfig.plist配置 ，可传空。 
       //isOpenInBody 是否开启体内配置，旧接口参数，2.0.0之后都需传入YES，否则没有广告位
       +(void)jcSDKInitConfigWithAppId:(NSString*)appId channelId:(NSString*)channelId isOpenInBody:(BOOL)isOpenInBody block:(void(^)(BOOL isOk))block;
       ```
   
   3. splash广告api：    
       ```
       //开屏请在window加载之后被调用
       [JC_iOSAdApi loadSplashView];
       ```
   
   4. banner广告api：  
       ```
       //推荐：先调用load进行广告位“预热” ，展示之前判断isReady是否为YES ，请自行设计调用场景，api最好不要连续，以免未及时load到数据
       [JC_iOSAdApi loadBannerConfig];

       BOOL isReady = [JC_iOSAdApi bannerIsReady]
       //con传入当前控制器即可
       [JC_iOSAdApi showBannerViewWithCon:con];
       ```
   
   5. Intersitial 广告 api：  
       ```
       ///推荐调用顺序 load - isReady - show - isReady - show（sdk内部采用了自动加载插屏资源功能，外部使用只需要调用一次load接口）
       [JC_iOSAdApi loadIntersitialConfig];

       BOOL isReady = [JC_iOSAdApi intersitialIsReady]

       [JC_iOSAdApi showIntersitialView];
       ```
   
   6. RewardView广告api：  
       ```
       //推荐调用顺序 load - isReady - show - isReady - show（sdk内部采用了自动加载激励视频资源功能，外部使用只需要调用一次load接口）
       [JC_iOSAdApi loadRewardConfig];
       BOOL isReady = [JC_iOSAdApi rewardVIsReady]
       [JC_iOSAdApi showRewardView];
       ```
   
   7. native 广告 api：  
       ```
       //native没有缓存池，每次使用调用load ，判断isReady后再展示。show方法有返回值，返回根据config生成的广告view 
       //JCNativeConfig 是native展示广告位的配置类，请配置完整，否则可能导致加载视图异常，请将返回的view加载到需要显示的视图上

       [JC_iOSAdApi loadNativeConfigSize:CGSizeMake(CGRectGetWidth(self.view.bounds), 350)]; //size：请和展示的原生view大小相同，避免加载不全

       BOOL isReady = [JC_iOSAdApi nativeIsReady]

       JCNativeConfig *config = [[JCNativeConfig alloc]init];
       config.ADFrame = CGRectMake(.0f, 200.0f, CGRectGetWidth(self.view.bounds), 350.0f);
       config.mediaViewFrame = CGRectMake(0, 120.0f, CGRectGetWidth(self.view.bounds), 350.0f - 120.0f);
       config.renderingViewClass = [[[CustomView alloc]init] class];
       config.rootViewController = self;
       UIView *adview = [JC_iOSAdApi showNativeConfigWithConfig:config];
       // 添加adview到视图上
       ```
   
   8. 广告回调 api：  
       ```
       //以下是splash广告的回调api使用示例，其他广告回调请自行使用.回调监听的key ，相关状态类型、回调参数说明请查看JCAdCallBackHeader.h类，请选择所需要的回调状态和参数进行监听和使用
       [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(msAdLoadCallBack:) name:MSSplashADKey object:nil];

       -(void)msAdLoadCallBack:(NSNotification*)noti{
           NSLog(@"%@",noti.userInfo);
           NSInteger code = [noti.userInfo[@"status"] integerValue];
           switch (code) {
               case MSAd_splashDidShow:
               {
                   NSLog(@"MSAd_splashDidShow");
               }
                   break;

               default:
                   break;
           }
       }
       ```
   
   9. 关于欧盟地区展示GDPR： 
       ```
       /// Determine if it is EU territory API
       /// @param block callback isEU? YES / NO
       +(void)getLocationIsEU:(void(^)(BOOL isEU))block;

       /// the GDPR interface API
       /// @param dismissblock close Interface callback
       /// @param failBlock show Fail callback
       +(void)jcSDKShowGDPRWithDismissblock:(void(^)(void))dismissblock loadFailblock:(void(^)(NSError *error))failBlock;
       ```
    10. Umeng 和 talkingData 数据上报：（如果你的项目中使用了umeng或者talkingdata，可以删除掉，采用sdk内部提供的umeng数据上报相关接口）  
         ```
         /// jsonStr Please convert the key-value to json.
         +(void)sendEvent:(NSString*)event detailedJsonString:(NSString*)jsonStr;
         ```
   </details>


- **常见报错处理:**
 
  <details>
  <summary>content</summary>

  1. 如果使用了快手SDK，在打包上传AppStore的时候，苹果不支持模拟器相关支持二进制，可以加入以下脚本，来删除模拟器相关二进制内容。  
  
        `APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"`  
        `find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK`  
        `do`  
        ` FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)`  
        ` FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"`  
        ` echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"`  
        ` EXTRACTED_ARCHS=()`  
        ` for ARCH in $ARCHS`  
        ` do`  
        `     echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"`  
        `     lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"`  
        `     EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")`  
        ` done`  
        ` echo "Merging extracted architectures: ${ARCHS}"`  
        ` lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"`  
        ` rm "${EXTRACTED_ARCHS[@]}"`  
        ` echo "Replacing original executable with thinned version"`  
        ` rm "$FRAMEWORK_EXECUTABLE_PATH"`  
        ` mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"`  
        ` done`  
  
  ![图片2]

  </details>

  
  

</details>
 
 ----
 
 ### English version
 
<details>
<summary>Detailed documentation</summary>
 
- **SDK Introduction：**  
 JCSDK is a set of advertising SDK provided by MS. It integrates the advertising SDKs of major advertisers and related data statistics SDKs to facilitate the joint operation and data analysis of in-app advertising between platforms.  
   1. Support ad types：  
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
 
- **SDK Access configuration:**  

   <details>
   <summary>content</summary>

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
       <key>GADIsAdManagerApp</key>
       <true/>
       
       Get location permission configuration
       <key>NSLocationWhenInUseUsageDescription</key>
       <string>The app needs to get your location</string>
       
       Get IDFA permissions ，iOS14support
       <key>NSUserTrackingUsageDescription</key> 
       <string>This identifier will be used to deliver personalized ads to you.</string>
       ```
   3. build setting configuration：  
        "bitcode" set "NO"  
        "other Linker Flags" set "-ObjC"  
   
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
   
- **SDK Api:**
   <details>
   <summary>content</summary>

   If there is a conflict between the API in the document and the API in the framework, please refer to the API in the framework.  
   
   1. header：
        #import <JCSDK/JCSDK.h>  
   
   2. init SDK：  
       ```
       //If appid and channelid are configured in JCiOSConfig.plist, they can be passed empty. 
       //isOpenInBody: Whether to open the body configuration (old interface parameters). After 2.0.0, YES must be passed in, otherwise there will be no advertising space
       +(void)jcSDKInitConfigWithAppId:(NSString*)appId channelId:(NSString*)channelId isOpenInBody:(BOOL)isOpenInBody block:(void(^)(BOOL isOk))block;
       ```
   
   3. splash api：    
       ```
       //Open the screen, splash be called after the window is loaded
       [JC_iOSAdApi loadSplashView];
       ```
   
   4. banner api：  
       ```
       //Recommendation: First call load to "warm up" the ad space, and judge whether isReady is YES before displaying. Please design the calling scene by yourself. The api is best not to be continuous, so as not to load the data in time
       [JC_iOSAdApi loadBannerConfig];

       BOOL isReady = [JC_iOSAdApi bannerIsReady]
       //con Just pass in the current controller
       [JC_iOSAdApi showBannerViewWithCon:con];
       ```
   
   5. Intersitial api：  
       ```
       ///Recommended calling sequence load-isReady-show-isReady-show (The automatic loading of interstitial resources is used inside the SDK, and the load interface only needs to be called once for external use)
       [JC_iOSAdApi loadIntersitialConfig];

       BOOL isReady = [JC_iOSAdApi intersitialIsReady]

       [JC_iOSAdApi showIntersitialView];
       ```
   
   6. RewardView api：  
       ```
       //Recommended calling sequence load-isReady-show-isReady-show (The function of automatically loading incentive video resources is used inside the SDK, and the load interface only needs to be called once for external use)
       [JC_iOSAdApi loadRewardConfig];
       BOOL isReady = [JC_iOSAdApi rewardVIsReady]
       [JC_iOSAdApi showRewardView];
       ```
   
   7. native api：  
       ```
       //Native does not have a buffer pool. Call load every time you use it, and then display it after judging isReady. The show method has a return value, which returns the ad view generated according to config 
       //JCNativeConfig is the configuration class of native display ad slots. Please configure it completely, otherwise it may cause abnormal loading of the view. Please load the returned view to the view that needs to be displayed

       //size：Please be the same size as the displayed native view to avoid incomplete loading
       [JC_iOSAdApi loadNativeConfigSize:CGSizeMake(CGRectGetWidth(self.view.bounds), 350)]; 

       BOOL isReady = [JC_iOSAdApi nativeIsReady]

       JCNativeConfig *config = [[JCNativeConfig alloc]init];
       config.ADFrame = CGRectMake(.0f, 200.0f, CGRectGetWidth(self.view.bounds), 350.0f);
       config.mediaViewFrame = CGRectMake(0, 120.0f, CGRectGetWidth(self.view.bounds), 350.0f - 120.0f);
       config.renderingViewClass = [[[CustomView alloc]init] class];
       config.rootViewController = self;
       UIView *adview = [JC_iOSAdApi showNativeConfigWithConfig:config];
       // add adview to superView
       ```
   
   8. ad callbcak api：  
       ```
       //The following is an example of using the callback api of the splash advertisement. For other advertisement callbacks, please use it yourself. The key for callback monitoring. Please refer to the "JCAdCallBackHeader.h" class for related status types and callback parameter descriptions. Please select the required callback status and parameters for monitoring and use

       [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(msAdLoadCallBack:) name:MSSplashADKey object:nil];

       -(void)msAdLoadCallBack:(NSNotification*)noti{
           NSLog(@"%@",noti.userInfo);
           NSInteger code = [noti.userInfo[@"status"] integerValue];
           switch (code) {
               case MSAd_splashDidShow:
               {
                   NSLog(@"MSAd_splashDidShow");
               }
                   break;

               default:
                   break;
           }
       }
       ```
   
   9. about GDPR： 
       ```
       /// Determine if it is EU territory API
       /// @param block callback isEU? YES / NO
       +(void)getLocationIsEU:(void(^)(BOOL isEU))block;

       /// the GDPR interface API
       /// @param dismissblock close Interface callback
       /// @param failBlock show Fail callback
       +(void)jcSDKShowGDPRWithDismissblock:(void(^)(void))dismissblock loadFailblock:(void(^)(NSError *error))failBlock;
       ```
    10. Umeng And talkingData send Message：(If you use umeng or talkingdata in your project, you can delete it and use the umeng data reporting interface provided by the sdk)  
       ```
        /// @param event event  
        /// @param jsonStr Please convert the key-value to json.  
        +(void)sendEvent:(NSString*)event detailedJsonString:(NSString*)jsonStr; 
       ```
   </details>


- **Common error handling:**
 
  <details>
  <summary>content</summary>

  1. If you use KSAdSDK, when you package and upload AppStore, Apple does not support the emulator-related support binary, you can add the following script to delete the emulator-related binary content.  
  
        `APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"`  
        `find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK`  
        `do`  
        ` FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)`  
        ` FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"`  
        ` echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"`  
        ` EXTRACTED_ARCHS=()`  
        ` for ARCH in $ARCHS`  
        ` do`  
        `     echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"`  
        `     lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"`  
        `     EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")`  
        ` done`  
        ` echo "Merging extracted architectures: ${ARCHS}"`  
        ` lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"`  
        ` rm "${EXTRACTED_ARCHS[@]}"`  
        ` echo "Replacing original executable with thinned version"`  
        ` rm "$FRAMEWORK_EXECUTABLE_PATH"`  
        ` mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"`  
        ` done`  
  
  ![图片2]

  </details>

  
  

</details>

