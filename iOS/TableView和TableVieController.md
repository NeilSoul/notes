iOS tableview controller

## stuff

```objc
//hide 
controller.hidesBottomBarWhenPushed = YES;
```

## some key

height of cell must be set.

* in nib
* in init method
* in delegate

## set in init

```objc
//same height of cell.

self.tableView.rowHeight = 70 ;

//header view
self.tableView.tableHeaderView = [[UIView alloc] initWithFrame:CGRectMake(0,0,5,20)]; //any view

//separator color
[self.tableView setSeparatorColor:[UIColor colorWithRed:1.0f green:1.0f blue:1.0f alpha:1.0]];
```

## customize  header

* set in init method

```objc
self.tableView.tableHeaderView = [[UIView alloc] initWithFrame:CGRectMake(0,0,5,20)]; //any view
```

* set in delegate method
```objc
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
    return 100.0;
}
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
{
    return @"some title";//must set title
}

- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    UIView *header = ..
    return _header;
}
```

## xib

### static table

* use storyboard, static cell
* use xib, define every cell view, and ctrl - drag into m file

## cell

### height
```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    switch (indexPath.section) {
        case NTOperationsSection:
            return kOperationCellHeight;
        case NTDetectedBeaconsSection:
        default:
            return kBeaconCellHeight;
    }
}
```
### cell view

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [staticCellArray objectAtIndex:[indexPath row]];
    return cell;
}
```

### reused cell and get point of object on cell

#### method 1 - storyboard

default ...

use accessory views,

#### method 2 - in delegate

tablecell xib , also set identifier
```objc
// Row display. Implementers should *always* try to reuse cells by setting each cell's reuseIdentifier and querying for available reusable cells with dequeueReusableCellWithIdentifier:
// Cell gets various attributes set automatically based on table (separators) and data source (accessory views, editing controls)

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *SimpleTableIdentifier = @"SimpleTableIdentifier";
    
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:
                             
                             SimpleTableIdentifier];
    
    if (cell == nil) {
        
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault
                 
                                       reuseIdentifier: SimpleTableIdentifier];
        
    }
    return cell;
}

```
#### method 3 - register in init method

in table init method
```objc
[tableView registerNib:[UINib nibWithNibName:@"RangeBeaconCell" bundle:nil]forCellReuseIdentifier:@"range_beacon_cell"];
//or register class
//[tableView registerClass[RangeBeaconCell class] ...];
```

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    RangeBeaconCell * cell = [tableView dequeueReusableCellWithIdentifier:@"range_beacon_cell"];
    return cell;
}
```


