# CoreDataViewControllers
[Core Data](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreData/cdProgrammingGuide.html) driven UITableViewController and UICollectionViewController.

### CDTableViewController
CoreDataTableViewController is created on [Stanford CS193p Winter 2013](http://www.stanford.edu/class/cs193p/cgi-bin/drupal/downloads-2013-winter) but this class mostly just copies the code from NSFetchedResultsController's documentation page into a subclass of UITableViewController.

### CDCollectionViewController
CoreDataCollectionViewController is conceptually the same as CDTableViewController but modified for use within UICollectionViewController.

### Usage
Just subclass the one you need (UITableViewController or UICollectionViewController) and set it's `NSFetchedResultsController *fetchedResultsController` property. 

After that you'll only have to implement `-tableView:cellForRowAtIndexPath:` or `-collectionView:cellForItemAtIndexPath:` and fetchedResultsController will take care of other required DataSource methods, and will also update UITableView or UICollectionView whenever the underlying data changes (insert, delete, update, move).

## Example
This example is for `CDTableViewController` but for `CDCollectionViewController` you only use `-collectionView:cellForItemAtIndexPath:` instead of `-tableView:cellForRowAtIndexPath:`.
```obj-c
- (void)viewDidLoad
{
    [super viewDidLoad];

    // set fetchedResultsController property
    [self refreshFetchedResultsController];
}

- (void)refreshFetchedResultsController
{
    // get NSManagedObjectContext
    NSManagedObjectContext *context = ...;
    
    if (context) {
        // create NSFetchRequest and NSFetchedResultsController
        NSFetchRequest *request = ...;
        NSSortDescriptor *sortDescriptor = ...;
        request.sortDescriptors = @[sortDescriptor];
        self.fetchedResultsController = [[NSFetchedResultsController alloc] initWithFetchRequest:request managedObjectContext:context sectionNameKeyPath:nil cacheName:nil];
    } else {
        self.fetchedResultsController = nil;
    }
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"CellIdentifier"];

    // get NSManagedObject from fetchedResultsController
    NSManagedObject *managedObject = [self.fetchedResultsController objectAtIndexPath:indexPath];
    
    // set cell data
    cell.textLabel.text = managedObject.someProperty;
    
    return cell;
}
```
