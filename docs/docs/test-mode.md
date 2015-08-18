## What is Test mode?
Test mode is designed for developer to test AD without mess up production AD serving environment. The AD in test mode won't show up in production AD list.

## How to create AD in test mode?
Operations can create test mode ADs just like the normal ADs, the only difference is to turn on Test mode AD flag in line item setting.

## What is the difference in test mode AD serving?
1. Test mode does not support geo targeting.

## How to enable Test mode in SDK?
Initialize SDK with test mode set to `YES`
```objc
// init SDK with test mode
[I2WAPI initWithVerboseLog:YES isTestMode:YES];
```
