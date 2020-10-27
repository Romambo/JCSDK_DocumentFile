
## If the access interface has changed, please see the version changes

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

