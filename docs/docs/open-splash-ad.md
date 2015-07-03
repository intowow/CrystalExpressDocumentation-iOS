## Trigger point
The timeing to request Open Splash AD is everytime App launch/enter from background.

## Supported AD format
The AD format that Open Splash support is **Splash series**

## Integration method
- In AppDelegate.m, request Open Splash while App launch/enter from background
    - Reference to sample code [AppDelegate.m](https://github.com/roylo/CrystalExpressSample/blob/master/CrystalExpressApp/CrystalExpressApp/AppDelegate.m)

### Integration detail
- While App launch, first initialize SDK, set `_shouldRequestOpenSplash` to YES
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/cbbc1fa02191568ceb86134afe7134488293e403/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L39)
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
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/cbbc1fa02191568ceb86134afe7134488293e403/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L65)
```objc
- (void)applicationWillEnterForeground:(UIApplication *)application
{
    _shouldRequestOpenSplash = YES;
    [I2WAPI refreshI2WAds];
}
```

- Don't request Open Splash in `applicationWillResignActive:` and `application:openURL:sourceApplication:annotation:`
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/cbbc1fa02191568ceb86134afe7134488293e403/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L47)
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
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/cbbc1fa02191568ceb86134afe7134488293e403/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L71)
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
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/cbbc1fa02191568ceb86134afe7134488293e403/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L115)
```objc
- (BOOL)requestOpenSplash
{
    if (_shouldRequestOpenSplash == NO) {
        return NO;
    } else {
        _shouldRequestOpenSplash = NO;
    }

    BOOL isShowingSplashAd = NO;
    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
        if ([topController isKindOfClass:[SplashADInterfaceViewController class]]) {
            isShowingSplashAd = YES;
            return NO;
        }
    }

    [_openSplashHelper requestSplashADWithPlacement:@"OPEN_SPLASH" mode:CE_SPLASH_MODE_SINGLE_OFFER];
    return YES;
}
```

- Implement SplashADHelper delegate function
    - `SplashADDidReceiveAd:viewController:` Success receive splash AD viewController, present it, then start to prepare App content viewController.
    - `SplashADDidFailToReceiveAdWithError:viewController:` Fail to request splash AD, start to prepare App content viewController.
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/cbbc1fa02191568ceb86134afe7134488293e403/CrystalExpressApp/CrystalExpressApp/AppDelegate.m#L138)
```objc
#pragma mark - SplashADHelperDelegate
- (void)SplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [topController presentViewController:vc animated:YES completion:^{
        [self prepareContentViewController];
    }];
}

- (void)SplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);
    [self prepareContentViewController];
}
```
