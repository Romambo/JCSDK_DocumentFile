[ios_unity_bridge]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/IOS_UnityBridge.zip
[iOS14 support]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/iOS14_support.md 
[JCSDK]: https://github.com/Romambo/JCSDK  
[DataCollenction_SDK]: https://github.com/Romambo/DataCollection_SDK  
[ADThirdParty_SDK]: https://github.com/Romambo/ADThirdParty_SDK  
[图片1]: https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/unity_image1.png
[图片2]:https://github.com/Romambo/JCSDK_DocumentFile/blob/main/imageFile/ios_image2.png
[下载链接]: https://drive.google.com/drive/folders/11N8YLnhyPVxv4HmdGZYni8o59S9aHp7M
[google download link]: https://drive.google.com/drive/folders/11N8YLnhyPVxv4HmdGZYni8o59S9aHp7M
[github download link]: https://github.com/Romambo/JCSDK_overseas  
[github 下载链接]: https://github.com/Romambo/JCSDK_overseas  

# JCSDK unity support document
 
 ### English version

   <details>
   <summary>Detailed documentation</summary>

   - **SDK Introduction：**  
          
        1. Version record：  

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

   - **SDK access process and configuration**  

       <details>
        <summary>content</summary>

       1. Download SDK library and required support library: [google download link] or [github download link]   
       
            
           File content description:(Put into unity)  
           iOS_UnityBridge : unity api  
             "IOSBridge.cs"、"IOSBridgeExtern.cs": Advertising api  
             "IOSListener.cs"、"IOSListenerExtern.cs": Advertising callback api  
             "JCiOSSDKSynchronizationContext.cs"、"OneThreadSynchronizationContext.cs": Multi-threaded optimization file  
             "JCiOSSDKPostprocess.cs": Access profile(Put into unity-Editor)  
      
           SDKFile:(Put into xcode project)  
             DataCollection_SDK ：  Some libraries about the data statistics platform  
             ADThirdParty_SDK ：    Some libraries about advertising platforms  
             MS_SDK ： 		           About our own JCSDK library   
          
       2. Access related ads APIs and callback Apis  
          
          You can see the following "SDK access process and configuration" and "Advertising interface callback API and use" to implement it yourself.We also provide cs files, you can refer to "iOS_UnityBridge"
       
       3. Xcode configuration  
           
           You can refer to the following list to configure manually. We also provide the "cs" file for reference. For details, see: "JCiOSSDKPostprocess.cs" in the "iOS_UnityBridge" file.(Please put JCiOSSDKPostprocess.cs in the Editor directory of Unity3D IDE. If there is an error in JCiOSSDKPostprocess.cs, please modify it yourself to adapt to the version.)  
           
           iOS14 support configuration details see [iOS14 support] document.  
           <details>
           <summary>configuration List</summary>

             1. xcode -> build setting configuration：  
       
                bitcode set "NO"  
                other Linker Flags set "-ObjC"  
             
             2. Add wifi permission  
       
                xcode -> target -> Signing&Capabilities . Upper left corner "+" Access WiFi Information  
             
             3. Import system support library：  
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
                
             4. info.pist configuration：  
                
                ```
                 Support http network configuration:
                 <key>NSAppTransportSecurity</key>
                 <dict>
                 <key>NSAllowsArbitraryLoads</key>
                 <true/>
                 </dict>

                 Google configuration:
                 <key>GADApplicationIdentifier</key>
                 <string>ca-app-pub-9488501426181082/7319780494</string>
                 <key>GADIsAdManagerApp</key>
                 <true/>

                 Get location permission configuration:
                 <key>NSLocationWhenInUseUsageDescription</key>
                 <string>The app needs to get your location</string>

                 Get IDFA permissions ，iOS14support:
                 <key>NSUserTrackingUsageDescription</key> 
                 <string>This identifier will be used to deliver personalized ads to you.</string>

                 <key>SKAdNetworkItems</key>
                 <array>
                  <dict>
                      <key>SKAdNetworkIdentifier</key>
                      <string>cstr6suwn9.skadnetwork</string>
                  </dict>
                  <dict>
                      <key>SKAdNetworkIdentifier</key>
                      <string>238da6jt44.skadnetwork</string>
                  </dict>
                  <dict>
                      <key>SKAdNetworkIdentifier</key>
                      <string>22mmun2rn5.skadnetwork</string>
                  </dict>
                  <dict>
                      <key>SKAdNetworkIdentifier</key>
                      <string>SU67R6K2V3.skadnetwork</string>
                  </dict>
                  <dict>
                      <key>SKAdNetworkIdentifier</key>
                      <string>4DZT52R2T5.skadnetwork</string>
                  </dict>
                  <dict>
                      <key>SKAdNetworkIdentifier</key>
                      <string>bvpn9ufa9b.skadnetwork</string>
                  </dict>
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
                  </dict>
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
                 </dict>
                 <dict>
                         <key>SKAdNetworkIdentifier</key>
                         <string>58922NB4GD.skadnetwork</string>
                     </dict>
                 <dict>
                         <key>SKAdNetworkIdentifier</key>
                         <string>V4NXQHLYQP.skadnetwork</string>
                     </dict>
                 <dict>
                         <key>SKAdNetworkIdentifier</key>
                         <string>GTA9LK7P23.skadnetwork</string>
                     </dict>
                </array>
            ```  
          ```
          
           </details>
   

        4. JCiOSConfig.plist Parameter Description：  Look at the downloaded "SDKFile" -> "MS_SDK" file  
        
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
             
        5. Export xcode project  
        
        6. Import the downloaded library file. Look at the downloaded "SDKFile" file  
        
           Right-click in the project and select "Add File to "you project"" to add the locally downloaded library file
       
           Some of these libraries are dynamic libraries，xcode -> target -> General -> Framework,Librares,and Embedded Content  
           Find the following library settings :(Embed & Sign):  
           > KSAdSDK.framework                   (Embed & Sign)    
           > KochavaCore.framework               (Embed & Sign)  
           > KochavaTracker.framework            (Embed & Sign)  
           > KochavaAdNetwork.framework          (Embed & Sign)  
           
           Then build your project, make sure there are no errors  
           
        7. Find UnityAppController.mm for initial access  
      
           1. Import header file
               ```
               #import <JCSDK/JCSDK>
               #import <AppTrackingTransparency/AppTrackingTransparency.h>
               ```
         
           2. Access initialization  
           
               find [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];  
               Replace startUnity: -> initSDKWithApplication:  
               [self performSelector: @selector(initSDKWithApplication:) withObject: application afterDelay: 0];  
          
               Add the following code  
             
                    ```
                    
                   -(void)initSDKWithApplication:(UIApplication*)application{
                     if (@available(iOS 14, *)) {
                         //iOS 14 System IDFA permission box
                         [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {

                             //2.0.0 init api
                             [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                                 [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                             }];
                         }];
                     } else {
                         //2.0.0 init api
                         [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                             [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                         }];
                     }
                   }
                 ``` 
         
      
       8. Add a script to process the emulator binary file in “KSAdSDK”, otherwise the package will report an error  
       
           xcode - target - Build Phases . Upper left corner “+” New Run script Phases  
           open "Run script"  
           Add the following script:  
        
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
              
              
        </details>
       

   - **unity Api ：**  

       <details>
       <summary>content</summary>

        If there is a conflict between the API in the document and the API in the framework, please refer to the API in the framework.
        
        1. init api：  

            ```
            V1.0.0 init api：
            -(void)initJCSDKWithLog:(BOOL)isOpenLog isFirstShowSplash:(BOOL)isShow splashClose:(unityBlock)block;

             V2.0.0 init api：
             -(void)initJCSDKWithUnityShow:(unityBlock)block;
            ```

        2. banner api：  
            ```
             /// isReady - banner
             bool isReadyBanner();

             /// show banner Ads
             void showBannerView();

             /// remove banner Ads
             void removeBannerView();
            ```

        3. Intersitial  api：  
            ```
             /// Intersitial Ads isReady
             bool isReadyIntersitial();

             /// show Intersitial Ads
             void showIntersitial();
            ```

        4. RewardView api：  
            ```
             /// rewardVideo Ads isReady
             bool isReadyRewardVideo();

             /// show rewardVideo Ads
             void showRewardVideo();
            ```
        5. Umeng And talkingData send Message：  
            ```
            /// Send Event UMeng、talkingData
            /// @param event event
            /// @param jsonEventInfo key-value converted json string, if there is no content to pass, you can set a null value
             void sendEvent(char *event,char *jsonEventInfo);
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

-------------------------

### 中文版本


<details>
<summary>详细文档</summary>
 
- **SDK接入配置和操作**  
  
  <details>
   <summary>content</summary>
  
   说明：接入所需支持： Xcode12 、iOS9.0
  
   1. 下载SDK库和所需支持库：[下载链接] 或者 [github 下载链接]  
   
      文件内容说明:  
         iOS_UnityBridge : unity api  
             "IOSBridge.cs"、"IOSBridgeExtern.cs": 广告API  
             "IOSListener.cs"、"IOSListenerExtern.cs": 广告回调API  
             "JCiOSSDKSynchronizationContext.cs"、"OneThreadSynchronizationContext.cs": 多线程优化文件  
             "JCiOSSDKPostprocess.cs": 配置文件  
      
         SDKFile:  
             DataCollection_SDK ：  数据统计三方支持库  
             ADThirdParty_SDK ：    广告三方支持库  
             MS_SDK ： 		           JCSDK和参数配置plist文件  
        
   2. 接入相关广告Api和回调Api：  
   
      
      详情请看下载的iOS_UnityBridge文件中的"IOSBridge.cs"和"IOSListener.cs"。如果想按照自己的方式接入的话，请往下看大目录"unity接入Api说明" 和 “广告接口回调Api和使用”。
      
      
   3. Xcod相关配置  
      可以打成xcode工程后，自己手动配置。  我们也提供了配置cs文件，可以参考使用 ，详情见iOS_UnityBridge文件中JCiOSSDKPostprocess.cs(如果JCiOSSDKPostprocess.cs中存在错误，请自行进行修改以适应该版本。 请将JCiOSSDKPostprocess.cs放入Unity3D IDE的Editor目录中)  
      
       <details>
       <summary>Xcod相关配置</summary>

       1. xcode -> build setting 配置：  
        bitcode 设置为NO  
        other Linker Flags 设置 -ObjC  

       2. 添加wifi权限  
          xcode -> target -> Signing&Capabilities 左上角 "+" Access WiFi Information  

       3. 导入系统支持库：  
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

       4. info.pist 配置：

           <details>
           <summary>配置列表</summary>

           ```
           支持http网络配置:
           <key>NSAppTransportSecurity</key>
           <dict>
           <key>NSAllowsArbitraryLoads</key>
           <true/>
           </dict>

           Google相关参数配置:
           <key>GADApplicationIdentifier</key>
           <string>ca-app-pub-9488501426181082/7319780494</string>
           <key>GADIsAdManagerApp</key>
           <true/>

           获取地理位置权限配置:
           <key>NSLocationWhenInUseUsageDescription</key>
           <string>The app needs to get your location</string>

           获取IDFA权限，iOS14支持:
           <key>NSUserTrackingUsageDescription</key> 
           <string>This identifier will be used to deliver personalized ads to you.</string>

           <key>SKAdNetworkItems</key>
           <array>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>cstr6suwn9.skadnetwork</string>
            </dict>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>238da6jt44.skadnetwork</string>
            </dict>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>22mmun2rn5.skadnetwork</string>
            </dict>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>SU67R6K2V3.skadnetwork</string>
            </dict>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>4DZT52R2T5.skadnetwork</string>
            </dict>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>bvpn9ufa9b.skadnetwork</string>
            </dict>
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
            </dict>
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
                </dict>
                <dict>
                        <key>SKAdNetworkIdentifier</key>
                        <string>58922NB4GD.skadnetwork</string>
                    </dict>
                <dict>
                        <key>SKAdNetworkIdentifier</key>
                        <string>V4NXQHLYQP.skadnetwork</string>
                    </dict>
                <dict>
                        <key>SKAdNetworkIdentifier</key>
                        <string>GTA9LK7P23.skadnetwork</string>
                    </dict>
           </array>
           ```
           </details>
       </details>
   
   
   4. JCiOSConfig.plist 参数说明：  
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
   
    5. 导出xcode工程  
    
    6. 导入下载好的库文件  
         工程内右键，选择“Add File to "you project"”来添加本地下载好的库文件  
         其中某些库是动态库，xcode -> target -> General -> Framework,Librares,and Embedded Content 找到以下库单独设置(Embed & Sign):  
         > KSAdSDK.framework                   (Embed & Sign)    
         > KochavaCore.framework               (Embed & Sign)  
         > KochavaTracker.framework            (Embed & Sign)  
         > KochavaAdNetwork.framework          (Embed & Sign)  
         
         然后build你的项目，确保没有报错  
         
    7. 找到UnityAppController.mm进行初始化接入  
      
      1. 导入头文件
         ```
         #import <JCSDK/JCSDK>
         #import <AppTrackingTransparency/AppTrackingTransparency.h>
         ```
         
       2. 接入初始化
          找到[self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];  
          替换掉startUnity: -> initSDKWithApplication:  
          [self performSelector: @selector(initSDKWithApplication:) withObject: application afterDelay: 0];  
          
          添加以下代码
           ```
          -(void)initSDKWithApplication:(UIApplication*)application{
            if (@available(iOS 14, *)) {
                //iOS 14 系统IDFA权限弹框
                [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {

                    //2.0.0初始化接口
                    [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                        [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                    }];
                }];
            } else {
                //2.0.0初始化接口
                [[JC_unityAdApi getInstance]initJCSDKWithUnityShow:^(BOOL showUnityTime) {
                    [self performSelector: @selector(startUnity:) withObject: application afterDelay: 0];
                }];
            }
          }
        ``` 
          
      
     8. 添加脚本处理KSAdSDK中的模拟器二进制，否则打包会报错  
        xcode - target - Build Phases 左上角 “+” New Run script Phases  
        打开 Run script  
        添加以下脚本：  
        
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
              
              
     </details>  
- **unity接入Api说明：**  

  <details>
  <summary>content</summary>

  如果文档内API和framework内API有冲突，请以framework内API为准。
   
  1. 初始化 api：  
      
       ```
       V1.0.0 初始化接口：
       -(void)initJCSDKWithLog:(BOOL)isOpenLog isFirstShowSplash:(BOOL)isShow splashClose:(unityBlock)block;

        V2.0.0 修改初始化接口：
        -(void)initJCSDKWithUnityShow:(unityBlock)block;
       ```
   
   2. banner广告api：  
       ```
        /// isReady - banner
        bool isReadyBanner();

        /// show banner Ads
        void showBannerView();

        /// remove banner Ads
        void removeBannerView();
       ```
   
   3. Intersitial 广告 api：  
       ```
        /// Intersitial Ads isReady
        bool isReadyIntersitial();

        /// show Intersitial Ads
        void showIntersitial();
       ```
   
   4. RewardView广告api：  
       ```
        /// rewardVideo Ads isReady
        bool isReadyRewardVideo();

        /// show rewardVideo Ads
        void showRewardVideo();
       ```
    5. Umeng 和 talkingData数据上报：  
       ```
       /// Send Event UMeng、talkingData
       /// @param event event
       /// @param jsonEventInfo key-value converted json string, if there is no content to pass, you can set a null value
        void sendEvent(char *event,char *jsonEventInfo);
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
 
