## Advanced configure setting in Splash AD
### Auto dismiss Splash AD after user engage
- You can automatically dismiss splash AD after user click on AD engage url.
- Need to set configure before `showFromViewController: animated:`

```objc
// set auto dismiss after user engage AD
[_CESplashAD setDismissAdAfterEngageAd:YES];

// need to set before this line
[_CESplashAD showFromViewController:topController animated:YES];
```
