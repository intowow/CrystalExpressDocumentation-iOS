## Requirements
This is target for Card format AD that is not used in UITableView, App has to control AD's behavior

## 1. Initialize CECardADHelper
Initialize helper with AD placement name
```objc
_adHelper = [[CECardADHelper alloc] initWithPlacement:@"STREAM"];
```

## 2. Request AD
```objc
    [_adHelper requestADonReady:^(ADView *adView) {
        // return ADView if request successfully, this is callback from main thread
        .....
    } onFailure:^(NSError *error) {
        // return error if request failed
        .....
    }];
```

## 3. Control AD's behavior (onShow/onHide/onStart/onStop)
After get ADView from CECardADHelper request, it is important for App to call API to control AD's behavior

1. While AD is present to user, should call `onShow`
2. While AD is disappear from user's view, should call `onHide`
3. When it needs to impression AD (start AD play), should call `onStart`
4. When it needs to stop AD play, should call `onStop`

```objc
// API in ADView
@interface ADView : UIView
/**
 *  call onShow while ADView is seen by user
 */
- (void)onShow;

/**
 *  call onHide while ADView is hide from user
 */
- (void)onHide;

/**
 *  call onStart to trigger ADView impression and video play
 */
- (void)onStart;

/**
 *  call onStop to stop ADView video play
 */
- (void)onStop;
@end
```

## Important reminders
1. Please avoid keeping ADs in memory for long time since it might out of AD's delivery time range.
2. Correctly control AD's behavior <br>
   For example, `onShow` should be called at the time user can see the AD, but not called while AD is added to view hierarchy.
   `onShow` `onHide` should be pair called
   `onStart` `onStop` should be pair called

***
More Information:

- [API reference](api-reference.md)
