In AppDelegate.m, register a background fetch will allow CrystalExpress SDK able to fetch ads while iOS awake app aperiodically.

## How to enable app background fetch?
![configure background fetch](../images/background_fetch.png)

1. In project settings, Target -> Capabilities -> Turn Background Modes to ON, check Background Fetch
2. In AppDelegate.m add function like the following code:

```objc
- (void)application:(UIApplication *)application performFetchWithCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
{
    [I2WAPI triggerBackgroundFetchOnSuccess:^{
        completionHandler(UIBackgroundFetchResultNewData);
    } onFail:^{
        completionHandler(UIBackgroundFetchResultFailed);
    } onNoData:^{
        completionHandler(UIBackgroundFetchResultNoData);
    }];

    ......
}
```
