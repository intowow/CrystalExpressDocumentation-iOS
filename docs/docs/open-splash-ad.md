## Trigger point
The timeing to request Open Splash AD is everytime App launch/enter from background.

## Supported AD format
The AD format that Open Splash support is **Splash series**

## Integration method
- In AppDelegate.m, request Open Splash while App launch/enter from background
    - Reference to sample code [AppDelegate.m](https://github.com/roylo/CrystalExpressSample/blob/master/CrystalExpressApp/CrystalExpressApp/AppDelegate.m)

### Integration detail
- While App launch, first initialize SDK, set `_shouldRequestOpenSplash` to YES
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L45)
```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    .....

    [I2WAPI initWithVerboseLog:YES isTestMode:NO];
    _shouldRequestOpenSplash = YES;

    .....
    return YES;
}
```

- While App will enter foreground from background, set `_shouldRequestOpenSplash` to YES
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L78)
```objc
- (void)applicationWillEnterForeground:(UIApplication *)application
{
    _shouldRequestOpenSplash = YES;
}
```

- Don't request Open Splash in `applicationWillResignActive:` and `application:openURL:sourceApplication:annotation:`
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L58)
```objc
- (void)applicationWillResignActive:(UIApplication *)application
{
    _shouldRequestOpenSplash = NO;
    .....
}

#pragma mark - deeplinking
- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation
{
    .....
    _shouldRequestOpenSplash = NO;
    return YES;
}
```

- Request Open Splash at `applicationDidBecomeActive:`
- If do not request Open Splash, then start to prepare App content viewController.
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L83)
```objc
- (void)applicationDidBecomeActive:(UIApplication *)application
{
    BOOL hasReqOpenSplash = [self requestOpenSplash];
    if (!hasReqOpenSplash) {
        [self prepareContentViewController];
    }
}
```

- First make sure there's no Splash AD on current UIView hierarchy, then request Open Splash
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L144)
```objc
- (BOOL)requestOpenSplash
{
    if (_shouldRequestOpenSplash == NO) {
        return NO;
    } else {
        _shouldRequestOpenSplash = NO;
    }

    [_openSplashHelper loadAd];
    return YES;
}
```

- Implement SplashADHelper delegate function
    - `CESplashADDidReceiveAd:viewController:` Success receive splash AD viewController, present it, then start to prepare App content viewController.
    - `CESplashADDidFailToReceiveAdWithError:viewController:` Fail to request splash AD, start to prepare App content viewController.
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L150)
```objc
#pragma mark - SplashADHelperDelegate
- (void)CESplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [_CEOpenSplashAD showFromViewController:topController animated:YES];
}

- (void)CESplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);
    [self prepareContentViewController];
}

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
