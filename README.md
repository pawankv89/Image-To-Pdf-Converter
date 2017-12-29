
PKImageToPdfConvert
=========

## PKImageToPdfConvert.
------------
 Added Some screens here.
 
[![](https://github.com/pawankv89/PKImageToPdfConverter/blob/master/Screens/1.png)]
[![](https://github.com/pawankv89/PKImageToPdfConverter/blob/master/Screens/2.png)]
[![](https://github.com/pawankv89/PKImageToPdfConverter/blob/master/Screens/3.png)]

## Usage
------------
 iOS 9 Demo showing how to droodown on iPhone X Simulator in  Objective-C.


```objective-c
- (void)viewDidLoad {
[super viewDidLoad];
// Do any additional setup after loading the view, typically from a nib.

self.assetsFetchResults = [NSMutableArray new];

//Configuration CollectionView Cell
[self collectionViewSetup];

[self loadDefaultTextOnWebView];
}

-(void)collectionViewSetup{

self.collectionView.dataSource = self;
self.collectionView.delegate = self;
[self.collectionView reloadData];
}


- (void)didReceiveMemoryWarning {
[super didReceiveMemoryWarning];
// Dispose of any resources that can be recreated.
}


#pragma mark - UICollectionViewDataSource

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
{
NSInteger count = self.assetsFetchResults.count;
return count;
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath{

GridViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"GridViewCell" forIndexPath:indexPath];

UIImage *image = [self.assetsFetchResults objectAtIndex:indexPath.row];
cell.thumbnailImage.image = image;

return cell;
}

- (UIImage *)imageFromColor:(UIColor *)color
{
CGRect rect = CGRectMake(0, 0, widthPDF, heightPDF);
UIGraphicsBeginImageContext(rect.size);
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextSetFillColorWithColor(context, [color CGColor]);
CGContextFillRect(context, rect);
UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
UIGraphicsEndImageContext();
return image;
}

- (UIColor *)randomColor
{
CGFloat hue = (arc4random() % 256 / 256.0);
CGFloat saturation = (arc4random() % 128 / 256.0) + 0.5;
CGFloat brightness = (arc4random() % 128 / 256.0) + 0.5;
return [UIColor colorWithHue:hue saturation:saturation brightness:brightness alpha:1];
}

- (IBAction)buttonImagePicker:(id)sender
{
//Clear Old Images Array
if ([self.assetsFetchResults count]>0){
[self.assetsFetchResults removeAllObjects];
}

for (NSInteger index = 0; index < imagesCountForPDF; index++) {

UIImage *image = [self imageFromColor:[self randomColor]];

[self.assetsFetchResults addObject:image];
}

//Complex operation that are not important
dispatch_async(dispatch_get_main_queue(), ^{

//Configuration
[self.collectionView reloadData];

});
}

- (IBAction)buttonConvertImageToPdf:(id)sender
{
//Clear Old Images Array
if ([self.assetsFetchResults count]>0){

NSArray *documentDirectories = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask,YES);
NSString *documentDirectory = [documentDirectories objectAtIndex:0];

NSString *aFilename = @"camera.pdf";
NSString *documentDirectoryFilename = [documentDirectory stringByAppendingPathComponent:aFilename];
NSLog(@"documentDirectoryFilename %@", documentDirectoryFilename);
UIGraphicsBeginPDFContextToFile(documentDirectoryFilename, CGRectMake(0, 0, widthPDF, heightPDF), nil);

// Render 10 UIImage objects to PDF file
for (NSInteger index = 0; index < [self.assetsFetchResults count]; index++) {
UIImage *image = [self.assetsFetchResults objectAtIndex:index];

@autoreleasepool {
UIGraphicsBeginPDFPage();
[image drawAtPoint:CGPointZero];
}
}

UIGraphicsEndPDFContext();

//Load WebView
NSURL *targetURL = [NSURL fileURLWithPath:documentDirectoryFilename];
NSURLRequest *request = [NSURLRequest requestWithURL:targetURL];
[self.webView loadRequest:request];

}else{

//Add Images First because your Array is empty
}
}

-(void)loadDefaultTextOnWebView{

NSString *htmlText = @"\nFrist Click on \nPick Images from Gallery \n Then Click on Convert Image To Pdf\n";
[self.webView loadHTMLString:[htmlText stringByReplacingOccurrencesOfString:@"\n" withString:@"<br/>"] baseURL:nil];

}


```

```objective-c

```

## License

This code is distributed under the terms and conditions of the [MIT license](LICENSE).

## Change-log

A brief summary of each this release can be found in the [CHANGELOG](CHANGELOG.mdown). 
