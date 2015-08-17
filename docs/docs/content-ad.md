## Requirements
- CEContentADHelper is designed for scrollView page

## 1. Initialize CEContentADHelper
- We provide CEContentADHelper to simplify content AD integration, through CEContentADHelper we can request/manage content AD
- While loading content page, setup a `UIView adWrapperView` as AD position, and pass it as a parameter while request AD
    - This `adWrapperView` need to set origin.y as AD position, set size width, the AD will place in the center of `adWrapperView`
- When initialize CEContentADHelper, set the AD placement name, scrollView and the content unique identifier

```objc
- (void)loadContentWithId:(NSString *)contentId
{
    // setup page content
      .....

    // content AD
    _adWrapperView = [[UIView alloc] initWithFrame:CGRectMake(0, scrollHeight, self.view.bounds.size.width, 0)];

    // setup page content below AD
      .....

    // you might need to set ScrollView content Size
    [_scrollView setContentSize:CGSizeMake(self.view.bounds.size.width, scrollHeight)];

    // setup content AD
    [self setupContentAdWithAdView:_adWrapperView contentId:contentId];
}

- (void)setupContentAdWithAdView:(UIView *)adView contentId:(NSString *)contentId
{
    _contentADHelper = [CEContentADHelper helperWithPlacement:@"CONTENT" scrollView:_scrollView contentId:contentId];

    // request AD
    [_contentADHelper loadAdInView:adView];
}
```

## 2. Update viewController show/hide from user
- While viewController is show in front of user, we need to call `onShow` to notify AD to play (ex. at `viewDidAppear`)
- While viewController is hide from user's view, we need to call `onHide` to notify AD to stop play (ex. at `viewDidDisappear`)

```objc
- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    [_contentADHelper onShow];
    .....
}

- (void)viewDidDisappear:(BOOL)animated
{
    [super viewDidDisappear:animated];
    [_contentADHelper onHide];
    .....
}
```
***
More Information

- [API reference](api-reference.md)
