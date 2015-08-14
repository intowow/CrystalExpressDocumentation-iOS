## Requirements
- App must support both portrait and landscape rotation.

## [Preview Splash AD in browser](https://s3.cn-north-1.amazonaws.com.cn/intowow-common/preview/SPLASH2_VIDEO_GENERAL_P_ICLICK.html)

## 1. Implement CESplashADDelegate in your class
```objc
@interface AppDelegate() <CESplashADDelegate>
```

## Init CESplashAD
- We provided a helper class to make integration more easier, via CESplashAD, you can easily request Splash ADs.
- Set AD placement name while initialize, and set delegate to self
    - `placement` placement name is used as an unique AD unit

```objc
// init splash helper
_CEOpenSplashAD = [[CESplashAD alloc] initWithPlacement:@"OPEN_SPLASH" delegate:self];
```

## Request Splash AD
```objc
// request AD
[_CEOpenSplashAD loadAd];
```

## CESplashAD delegate
CESplashAD will call delegate function and return a ready `SplashADInterfaceViewController` for you to present.
```objc
#pragma mark - CESplashADDelegate

- (void)CESplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{

    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [_CEOpenSplashAD showFromViewController:topController animated:YES];
}
```
If request Splash AD fail, CESplashAD will call this function and return the error.
```objc
- (void)CESplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);

    // direct to your content viewcontroller
    .....
}
```

You can implement the following delegate functions to receive extra events.
```objc
// splash ad viewcontroller is going to dismiss from screen
- (void)CESplashAdWillDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller is goint to present on screen
- (void)CESplashAdWillPresentScreen:(SplashADInterfaceViewController *)vc
{

}

// splash ad viewcontroller did dismiss from screen
- (void)CESplashAdDidDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller did present to screen
- (void)CESplashAdDidPresentScreen:(SplashADInterfaceViewController *)vc
{
    // prepare your app view controller
    ....
}
```
**Notice** There are both portrait and landscape Splash AD support in our SDK, make sure your app support that kind of rotation before you serving ADs.

## Suggested Splash AD placement
- [Open Splash](open-splash-ad.md)
- [Interstitial Splash](interstitial-ad.md)

***
More information

- [API reference]()
