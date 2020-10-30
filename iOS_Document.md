[版本记录]: https://developer.apple.com/documentation/apptrackingtransparency?language=objc  
[iOS14 support]: 
[JCSDK]  
[DataCollenction_SDK]  
[ADThirdParty_SDK]  

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
   [图片1]    
   V2.0.0 新增  
   | KochavaAppID  | kochava初始化所需的appid | 
   | TenJinAppID  | kochava初始化所需的appid |
   | ShowSplashFirst  | kochava初始化所需的appid |
   | LogLevel  | 日志等级：字符串1、关闭。2、开JC日志。3、开JC+ad日志。4、开JC+ad+data 日志 |
   
- SDK相关Api:  

- 常见报错处理:  


</details>
 
 ----
 
 ### English version
 
 <details>
<summary>Detailed description</summary>


</details>

