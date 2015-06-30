## Requirements
Stream ADs are designed for UITableView class

## Init streamADHelper
- We provided a helper class to make stream AD integration easier, via StreamADHelper, you can request and manage stream ADs.
- Initial streamADHelper in the pre stage (ex. `viewDidLoad`)
- `preroll` may prepare 1 stream AD to insert in data source at very begining position. Call this before `[self.tableView reloadData]`
```objc
- (void)viewDidLoad
{
    .....

    // prepare your tableview data source
    [self prepareDataSource];
    if (_streamHelper) {
        // set helper delegate to get AD event
        [_streamHelper setDelegate:self];

        // if you need customized ad width, add this line (Optional)
        [_streamHelper setPreferAdWidth:320.0f];

        // call preroll to prepare 1 stream AD before tableView load data
        [_streamHelper preroll];
    }
    [self.tableView reloadData];

    // update cell visible position allow helper to check whether AD should start/stop
    [_streamHelper updateVisiblePosition:[self tableView]];

   .....
}
```

## Request stream AD
StreamADHelper will return whether indexPath is a stream AD, and return the corresponding ADView.
```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // check whether indexPath is a stream AD
    UITableViewCell *cell = [self getADTableViewCellForTableView:tableView atIndexPath:indexPath];
    if (cell != nil) {
        return cell;
    } else {
        // return the normal content cell
        .....
    }
}

- (UITableViewCell *)getADTableViewCellForTableView:(UITableView *)tableView atIndexPath:(NSIndexPath *)indexPath
{
    UIView *adView = [_streamHelper requestADAtPosition:indexPath];
    if (adView != nil) {
        NSString *identifier = [NSString stringWithFormat:@"ADCell_%@_%d_%d", _sectionName, (int)[indexPath section], (int)[indexPath row]];
        UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:identifier];
        if (!cell) {
            cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:identifier];
            [cell setSelectionStyle:UITableViewCellSelectionStyleNone];
        }

        [[cell contentView] addSubview:adView];
        [adView setFrame:CGRectMake((SCREEN_WIDTH - adView.bounds.size.width)/2.0f, _adVerticalMargin, adView.bounds.size.width, adView.bounds.size.height)];
        [[cell contentView] setBackgroundColor:[UIColor colorWithWhite:0.905 alpha:1.0]];
        return cell;
    } else {
        return nil;
    }
}
```

## StreamADHelper delegate
### onADLoaded
- While stream AD view is ready, SDK will callback to `onADLoaded:(UIView *) atIndexPath:(NSIndexPath *) isPreroll:(BOOL)`
    - `indexPath` indicate the target indexPath SDK prefer to insert
    - `isPreroll` indicate whether this is a preroll request.
- If it is a preroll request, there's no need to insert data source in next main loop.
- Return the real inserted NSIndexPath to helper, or nil if fail to insert.
```objc
- (NSIndexPath *)onADLoaded:(UIView *)adView atIndexPath:(NSIndexPath *)indexPath isPreroll:(BOOL)isPreroll
{
    // Don't place ad at the first place!!
    int position = MAX(1, [indexPath row]);
    NSMutableArray *dataSource = [_dataSources objectAtIndex:[indexPath section]];
    NSIndexPath *finalIndexPath = [NSIndexPath indexPathForRow:position inSection:[indexPath section]];

    if ([dataSource count] >= position) {
        // if this request is preroll, no need to insert in another main loop
        if (isPreroll) {
            NSMutableDictionary *adDict = [[NSMutableDictionary alloc] init];
            CGFloat adHeight = adView.bounds.size.height;
            [adDict setObject:[NSNumber numberWithFloat:adHeight + 2*_adVerticalMargin] forKey:@"height"];

            NSArray *indexPathsToAdd = @[finalIndexPath];
            [[self tableView] beginUpdates];
            [dataSource insertObject:adDict atIndex:position];
            [[self tableView] insertRowsAtIndexPaths:indexPathsToAdd
                                    withRowAnimation:UITableViewRowAnimationNone];
            [[self tableView] endUpdates];
        } else {
            dispatch_async(dispatch_get_main_queue(), ^(){
                NSMutableDictionary *adDict = [[NSMutableDictionary alloc] init];
                CGFloat adHeight = adView.bounds.size.height;
                [adDict setObject:[NSNumber numberWithFloat:adHeight + 2*_adVerticalMargin] forKey:@"height"];

                NSArray *indexPathsToAdd = @[finalIndexPath];
                [[self tableView] beginUpdates];
                [dataSource insertObject:adDict atIndex:position];
                [[self tableView] insertRowsAtIndexPaths:indexPathsToAdd
                                        withRowAnimation:UITableViewRowAnimationNone];
                [[self tableView] endUpdates];
            });
        }

        // return the real indexPath inserted into tableView
        return finalIndexPath;
    } else {
        // return nil if error
        return nil;
    }
}
```

### onADAnimation
- onADAnimation will only be called with a specific AD format (Card-Video-PullDown)
- When the AD is clicked by user, the engage module will extend from the AD view's bottom. Therefore, tableView should update cell height for the animation.
- [TODO] 補圖
```objc
- (void)onADAnimation:(UIView *)adView atIndexPath:(NSIndexPath *)indexPath
{
    NSMutableArray *dataSource = [_dataSources objectAtIndex:[indexPath section]];
    [UIView animateWithDuration:1.0 delay:0.0 options:UIViewAnimationOptionAllowUserInteraction animations:^{
        [[self tableView] beginUpdates];
        [[dataSource objectAtIndex:[indexPath row]] setObject:[NSNumber numberWithInt:adView.bounds.size.height + 2*_adVerticalMargin] forKey:@"height"];
        [[self tableView] endUpdates];
    } completion:^(BOOL finished) {

    }];
}
```
### Check whether tableView is in idle status
