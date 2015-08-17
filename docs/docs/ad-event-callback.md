CrystalExpress SDK provide AD event callback in AD impression and click, integration steps are listed as follows:

## 1. Implement `I2WADEventDelegate` in your class
```objc
#import "I2WAPI.h"

// implement I2WADEventDelegate method
@interface AppDelegate() <I2WADEventDelegate>
```

## 2. After initialize I2WAPI, set AD event delegate to self
```objc
// init I2WAPI
[I2WAPI initWithVerboseLog:NO isTestMode:NO];

// set delegate to self
[I2WAPI setAdEventDelegate:self];
```

## 3. Implement AD callback functions
- The callback function will have AD identifier string to distinguish different AD
- **Notice** the callback functions are not gurantee to call from main thread. If you need operate UI elements, please do it on main thread.

```objc
#pragma mark - adEvent delegate
- (void)onAdClick:(NSString *)adId data:(NSString *)data
{
    dispatch_async(dispatch_get_main_queue(), ^{
        NSLog(@"ad[%@] click!", adId);
    });
}

- (void)onAdImpression:(NSString *)adId
{
    dispatch_async(dispatch_get_main_queue(), ^{
        NSLog(@"ad[%@] impression!", adId);
    });
}
```
