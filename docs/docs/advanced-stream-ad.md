## Advanced configure setting in stream AD

### Auto play control

`CETableViewADHelper.h` provides 3 APIs to control stream AD start/stop play

1. Disable AD auto play. After disable, all stream ADs will show image (video cover image in video AD) while show up
2. Start AD play at specific indexPath
3. Stop AD play at specific indexPath

```objc
// first init CETableViewADHelper
_adHelper = [CETableViewADHelper helperWithTableView:self.tableView viewController:self placement:@"STREAM"];

// 1. disable AD auto play
[_adHelper setAutoPlay:NO];

// 2. start AD play at NSIndexPath
[_adHelper startAdAtIndexPath:indexPath];

// 3. stop AD play at NSIndexPath
[_adHelper stopAdAtIndexPath:indexPath];
```
---

### UI setting control
`CETableViewADHelper.h` provides the following APIs to adjust UI for stream AD

1. Set stream AD's width
2. Set the vertical margin (top & bottom) of stream AD
3. Set the background color of stream AD
4. Customized UI of stream AD

```objc
/**
 *  @optional
 *  this is an optional method to set stream AD's width
 *  stream AD's height will be adjusted to keep the original creative ratio from width
 *
 *  @param width stream AD width
 */
- (void)setAdWidth:(float)width;

/**
 *  @optional
 *  this is an optiona; method to set stream AD's vertical margin between cells
 *
 *  @param verticalMargin the margin height
 */
- (void)setAdVerticalMargin:(float)verticalMargin;

/**
 *  @optional
 *  this is an optional method to set stream AD's background color
 *
 *  @param bgColor background color
 */
- (void)setAdBackgroundColor:(UIColor *)bgColor;

/**
 *  @optional
 *  this is an optional method to customized adCell view after ADView is set in UITableViewCell
 *
 *  @param customizedAdCellBlock customized code block
 */
- (void)setAdCellCustomizedBlock:(void (^)(UITableViewCell *adCell))customizedAdCellBlock;
```
---

### Put road block Ad in stream
1. Please contact Intowow to do the road block Ad setting and the Ad placement configration.
2. Initialize `CETableViewADHelper` with **`adTag`** method
    - `adTag` is a string and also an identifier for the stream

```objc
  // use adTag instead of placement string
  _adHelper = [CETableViewADHelper helperWithTableView:self.tableView viewController:self adTag:adTag];
```

---

### Disable stream Ad in runtime
- `CETableViewADHelper` can disable loading Ad (also clean the current Ads) by calling the following API.

```objc
/**
 *  disable load AD
 */
- (void)disableAd;
```
