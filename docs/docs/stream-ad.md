## Requirements
- The Stream AD integration is designed based on UITableViewController

## 1. Initialize CETableViewADHelper
- We provide CETableViewADHelper class to simplify stream AD integration process, through CETableViewADHelper, you can request and manage stream ADs
- Initialize CETableViewADHelper at object init stage (ex. `viewDidLoad`)
```objc
- (void)viewDidLoad
{
    .....

    // prepare your tableview data source
    [self prepareDataSource];

    //
    [self setupStreamADHelper];
   .....
}

#pragma mark - stream ADHelper
- (void)setupStreamADHelper
{
    _adHelper = [CETableViewADHelper helperWithTableView:self.tableView viewController:self placement:@"STREAM"];

    // ---- @optional block ------------------------------
    // You can assign a AD width for stream AD
    [_adHelper setAdWidth:310];

    // You can assign background color for AD cell
    [_adHelper setAdBackgroundColor:[UIColor colorWithWhite:0.906 alpha:1.0f]];

    // You can assign vertical margin for AD cell
    [_adHelper setAdVerticalMargin:5.0f];

    // ---- end of optional block ------------------------

    // Configure all the setting before loadAd
    // start request AD
    [_adHelper loadAd];
}

```

## 2. Update viewController show/hide from user
- While viewController is show in front of user, we need to call `onShow` to notify AD to play (ex. at `viewDidAppear`)
- While viewController is hide from user's view, we need to call `onHide` to notify AD to stop play (ex. at `viewDidDisappear`)

```objc
- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    [_adHelper onShow];
}

- (void)viewDidDisappear:(BOOL)animated
{
    [super viewDidDisappear:animated];
    [_adHelper onHide];
}
```

## 3. Replace `UITableView` call methods
- Please replace all the following `UITableView` methods with the CrystalExpressSDK category equivalent methods

For example, the origin method is:

```objc
[self.tableView selectRowAtIndexPath:myIndexPath];
```

replace it into:

```objc
[self.tableView ce_selectRowAtIndexPath:myIndexPath];
```

These methods are just the same with normal `UITableView` method, but to adjust `NSIndexPath` based on the inserted ADs position

`**Important**` The replacement of these methods are crucial, if you skip this integration step, the content cell position in app might be wrong

| Original method                                   | Replace method                                       |
|---------------------------------------------------|------------------------------------------------------|
| setDelegate:                                      | ce_setDelegate:                                      |
| delegate                                          | ce_delegate                                          |
| setDataSource:                                    | ce_setDataSource:                                    |
| dataSource                                        | ce_dataSource                                        |
| reloadData                                        | ce_reloadData                                        |
| rectForRowAtIndexPath:                            | ce_rectForRowAtIndexPath:                            |
| indexPathForRowAtPoint:                           | ce_indexPathForRowAtPoint:                           |
| indexPathForCell:                                 | ce_indexPathForCell:                                 |
| indexPathsForRowsInRect:                          | ce_indexPathsForRowsInRect:                          |
| cellForRowAtIndexPath:                            | ce_cellForRowAtIndexPath:                            |
| visibleCells                                      | ce_visibleCells                                      |
| indexPathsForVisibleRows:                         | ce_indexPathsForVisibleRows:                         |
| scrollToRowAtIndexPath:atScrollPosition:animated: | ce_scrollToRowAtIndexPath:atScrollPosition:animated: |
| beginUpdates                                      | ce_beginUpdates                                      |
| endUpdates                                        | ce_endUpdates                                        |
| insertSections:withRowAnimation:                  | ce_insertSections:withRowAnimation:                  |
| deleteSections:withRowAnimation:                  | ce_deleteSections:withRowAnimation:                  |
| reloadSections:withRowAnimation:                  | ce_reloadSections:withRowAnimation:                  |
| moveSection:toSection:                            | ce_moveSection:toSection:                            |
| insertRowsAtIndexPaths:withRowAnimation:          | ce_insertRowsAtIndexPaths:withRowAnimation:          |
| deleteRowsAtIndexPaths:withRowAnimation:          | ce_deleteRowsAtIndexPaths:withRowAnimation:          |
| reloadRowsAtIndexPaths:withRowAnimation:          | ce_reloadRowsAtIndexPaths:withRowAnimation:          |
| moveRowAtIndexPath:toIndexPath:                   | ce_moveRowAtIndexPath:toIndexPath:                   |
| indexPathForSelectedRow:                          | ce_indexPathForSelectedRow:                          |
| indexPathsForSelectedRows:                        | ce_indexPathsForSelectedRows:                        |
| selectRowAtIndexPath:animated:scrollPosition:     | ce_selectRowAtIndexPath:animated:scrollPosition:     |
| deselectRowAtIndexPath:animated:                  | ce_deselectRowAtIndexPath:animated:                  |
| dequeueReusableCellWithIdentifier:forIndexPath:   | ce_dequeueReusableCellWithIdentifier:forIndexPath:   |

## 4. Refresh tableView data source
It is common for tableView to refresh data source periodically (ex. pull to refresh). While the data souce is refeshing, at the mean time, CETableViewADHelper is also need to clean previous cached ADs
```objc
- (void)refresh
{
    [self.pullToRefreshView startLoading];
    [self prepareDataSources];
    [_adHelper cleanAds];
    [self.tableView reloadData];
    [self.pullToRefreshView finishLoading];
}

- (void)pullToRefreshViewDidStartLoading:(SSPullToRefreshView *)view
{
    [self refresh];
}
```
***
More information

- [API reference](api-reference.md)
