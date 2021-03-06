## Requirements
- CrystalExpress works on iOS 7.0 and above.

## Before SDK integration
- If you don't have `Crystal_Id`, please contact Intowow to request one for you app.

## Installation
### Using Cocoapods
- We strongly recommand you to use Cocoapods to integrate with CrystalExpress.
- Add the following code in Podfile
```
pod "CrystalExpressSDK", '~> 1.6'
```
- `pod update` or `pod install`
- Open workspace that pod generate for you, you're ready to use CrystalExpress
- Here's a [sample project](https://github.com/ytli1204/CrystalExpressSample)

### Manual integration
1. In project build phases "Link Binary With Libraries", add CrystalExpressSDK-x.x.x.a static library
    - [CrystalExpressSDK-1.8](https://s3-ap-northeast-1.amazonaws.com/intowow-sdk/ios/manual/global/CrystalExpressSDK-1.8.0.zip)
2. Add source files to your project
3. Make sure you have the following frameworks added in Build phases
    - Security.framework
    - CFNetwork.framework
    - MessageUI.framework
    - MobileCoreServices.framework
    - SystemConfiguration.framework
    - AdSupport.framework
    - libz.dylib
    - libc++.dylib
    - CoreTelephony.framework
    - CoreMedia.framework
    - libsqlite3.dylib
    - AVFoundation.framework
    - libicucore.dylib
4. Add `-ObjC` in TARGETS -> Build Settings -> Linking -> Other Linker Flags
5. You can now start using CrystalExpress lib.

## Allow Http connection
- After xcode7 (iOS9 SDK), it only allow https connections, so we need to add the following settings in your app Info.plist to pass the Https check.
- Since we don't know what AD's click through url is, we need to allow arbitrary loads for Http connection.
```xml
<key>NSAppTransportSecurity</key>
<dict>
     <key>NSAllowsArbitraryLoads</key>
     <true/>
</dict>
```

##Whitelist Specific Apps
- After xcode7 (iOS9 SDK), if you want to launch third party App from your App, you need to add the following settings in your app Info.plist that can allow to check specific app installed or open brower, e.g., we want to open the facebook app with url "fb://profile/1493535890869". You can get more detail by checking https://developer.apple.com/videos/wwdc/2015/?id=703.
```xml
<key>LSApplicationQueriesSchemes</key>
<array>
		<string>fb</string>
		<string>twitter</string>
		<string>youtube</string>
		<string>...</string>
</array>
```