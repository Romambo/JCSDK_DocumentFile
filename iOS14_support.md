[App Tracking Transparency]: https://developer.apple.com/documentation/apptrackingtransparency?language=objc
# iOS 14 Support

## 中文版本

<details>
<summary>中文版本</summary>

### 概述  
从iOS 14开始，只有在获得用户明确许可的前提下，应用才可以访问用户的IDFA数据并向用户投放定向广告。在应用程序调用[App Tracking Transparency]框架向最终用户提出应用程序跟踪授权请求之前，IDFA将不可用。如果某个应用未提出此请求，则读取到的IDFA将返回全为0的字符串。本指南将介绍iOS 14支持所需的更改。

</details>
 

