## Trigger point
While back to stream section page from detail content page.

## Supported AD format
The AD format that Interstitial Splash support is **Splash series**

## Integration method
- Reference to [sample code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/ViewController/DemoStreamSectionViewController.m)

### Integration detail
- In sample project, `DemoDetailContentViewController` is detail content page, `DemoStreamSectionViewController` is stream section page
- While user press back button in `DemoDetailContentViewController`, request `DemoStreamSectionViewController` requestInterstitialSplashAD method after pop viewController animation done.
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/ViewController/DemoDetailContentViewController.m#L148)
```objc
#pragma mark - DemoDetailContentViewController.m
- (void)pressBackBtn:(id)sender
{
    [CATransaction begin];
    [CATransaction setCompletionBlock:^{
        [_delegate requestInterstitialSplashAD];
    }];

    [self.navigationController popViewControllerAnimated:YES];

    [CATransaction commit];
}
```

- Implement requestInterstitialSplashAD function in `DemoStreamSectionViewController`
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/ViewController/DemoStreamSectionViewController.m#L231)
- Implement CESplashADDelegate functions
    - [View code](https://github.com/roylo/CrystalExpressSample/blob/6f9db77f1cc4a1d05a602bcb82096937290d7aef/CrystalExpressApp/CrystalExpressApp/ViewController/DemoStreamSectionViewController.m#L236)
```objc
#pragma mark - DemoStreamSectionViewController.m
- (instancetype)init
{
    self = [super init];
    if (self) {
        ....
        _CEInterstitialSplash = [[CESplashAD alloc] initWithPlacement:@"INTERSTITIAL_SPLASH" delegate:self];
    }
    return self;
}

- (void)requestInterstitialSplashAD
{
    [_interstitialSplashHelper loadAd];
}

- (void)CESplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    [_CEInterstitialSplash showFromViewController:self animated:YES];
}

- (void)CESplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog("request interstitial AD fail, error:%@", error);
}

- (void)CESplashAdWillDismissScreen:(SplashADInterfaceViewController *)vc
{
}

- (void)CESplashAdWillPresentScreen:(SplashADInterfaceViewController *)vc
{
}

- (void)CESplashAdDidDismissScreen:(SplashADInterfaceViewController *)vc
{
}

- (void)CESplashAdDidPresentScreen:(SplashADInterfaceViewController *)vc
{
}
```
