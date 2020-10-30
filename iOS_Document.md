[版本记录]: https://developer.apple.com/documentation/apptrackingtransparency?language=objc  
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
   请参阅 [版本记录]  
 
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
   </details>


- **常见报错处理:**
 
  <details>
  <summary>content</summary>

  1. 如果使用了快手SDK，在打包上传AppStore的时候，苹果不支持模拟器相关支持二进制，可以加入以下脚本，来删除模拟器相关二进制内容。
  
  ```
   APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"
   find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
   do
    FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
    FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
    echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"

    EXTRACTED_ARCHS=()

    for ARCH in $ARCHS
    do
        echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"
        lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
        EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
    done

    echo "Merging extracted architectures: ${ARCHS}"
    lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
    rm "${EXTRACTED_ARCHS[@]}"

    echo "Replacing original executable with thinned version"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"

   done
  ```
  
  ![图片2]

  </details>

  
  

</details>
 
 ----
 
 ### English version
 
 <details>
<summary>Detailed description</summary>


</details>

