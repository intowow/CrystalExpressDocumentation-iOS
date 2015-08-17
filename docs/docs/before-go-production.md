## Checklist for App release
### SDK integration related
Please make sure the following functions are integrated in App before release.

1. [App has register background task](register-background-task.md)
2. [App has register background fetch](register-background-fetch.md)
3. App has deeplink, and tell Intowow the URL scheme
    - [Enable AD preivew function](ad-preview.md)

### AD integration related
1. Initialized SDK with turn off verbose log and test mode <br>
`[I2WAPI initWithVerboseLog:NO isTestMode:NO];`
2. Use correct Crystal_id represent your App, not testing Crystal_id
