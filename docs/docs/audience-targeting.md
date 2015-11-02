## Support audience targeting
To enable support AD audience targeting, you need to set user tags with the following API in `I2WAPI.h`

```objc
// I2WAPI.h
/**
 *  @brief set audience targeting tags
 *
 *  @param tags set of tag strings
 */
+ (void)setAudienceTargetingUserTags:(NSSet *)tags;
```

- `NSSet *tags` contains a set of Strings representing the user's information.
- By setting user tags, CrystalExpressSDK will prefetch and display the AD which matches the user tags.
- SDK will store last setting of user tags, no need to reset user tags every time App launch.

### Best practice
- Set the user tags as early as possible, SDK will have more time to prepare the right AD.
- Avoid changing user tags in runtime oftenly, it might decrease the fill rate of ADs.
