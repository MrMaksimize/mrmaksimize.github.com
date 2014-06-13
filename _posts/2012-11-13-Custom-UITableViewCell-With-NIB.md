---
layout: post
title: "Creating Custom UITableViewCells with NIB files"
description: ""
category: ios
tags: [code, ios]
comments: true
share: true
---



Well this sucks.  Apparently these days you can only use the Interface Builder to design your cell in XCode if you're using Storyboards.  But no worries. I have found a workaround which plays very nicely in iOS5+.  Let's get to it!

I'm assuming you already know so iOS and objective C, so I'll save the prep for another blog post.  Let's get down to business.

<!--break-->

You have your TableViewController.h and .m files ready. Now you want to create a sleek custom cell.

Create a new file (File -> New -> Objective C Class -> Subclass of UITableViewCell).  Note that the option to create a XIB for user interface is not available.  That's fine for now.  Let's call this class `EXCustomCell`.

This will get XCode to create 2 files - EXCustomCell.h and EXCustomCell.m.

In the EXCustomCell.h file, right below @interface line, add

    @property (nonatomic, strong) IBOutlet UILabel *cellItemLabel;
    @property (nonatomic, strong) IBOutlet UIImageView *cellItemImageView;

These are the properties of the cell.  Add more to your liking!  For every one that you want to add in IB and control programmatically, you'll need one.


I'm assuming you're using the latest version of XCode, so you won't need to @synthesize.  But just in case you're not on the latest,

    @synthesize cellItemImageView, cellItemLabel;

right under the @implementation line at the top of the EXCustomCell.m file.

Ok, now we've done all the setup work, time for some IB fun!

Go to File -> New, in the left hand column select User Interface, and pick View.  Leave iPhone for Device Family and title the file EXCustomCell.xib

![Select View](/assets/images/blog_post_images/custom_uitableviewcell/add_view.png)

Click on the file to open up Interface Builder.

In the left hand column, under objects, click once on View and hit the delete button on your keyboard.
![Remove View](/assets/images/blog_post_images/custom_uitableviewcell/remove_view.png)

Drag in a Table View Cell from the Object Library

Click on File's Owner, pick the Identity Panel, and make sure that NSObject is selected as the custom class.

Click on the Cell you just dragged in and make sure its custom class is EXCustomCell

Drag in an ImageView and a Label.

This is important.  Right click on the Custom cell under objects, pick the cellItemImageView and drag it over to the image, and repeat the same for cellItemLabel (except drag it to the label).

![Drag IBACTION](/assets/images/blog_post_images/custom_uitableviewcell/drag_ibaction.png)

Add #import "EXCustomCell.h" to your TableViewController.m file

Now in your TableViewController.m file, at the viewDidLoad method:

    - (void)viewDidLoad
    {
        [super viewDidLoad];
        [self.tableView registerNib:[UINib nibWithNibName:@"EXCustomCell"
                             bundle:[NSBundle mainBundle]]
             forCellReuseIdentifier:@"CustomCellReuseID"];
    }

Note that  `- (void) registerNib:(UINib*)nib forCellReuseIdentifier:(NSString*)identifier;` is an iOS 5+ method.  So be careful!


And in cellForRowAtIndexPath:

    - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
    {
        static NSString *CellIdentifier = @"CustomCellReuseID";
        EXCustomCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];

        // Configure the cell...
        [cell.cellItemImage setImage:[UIImage imageNamed:@"glyphicons_428_podium"]];
        [cell.cellItemLabel setText = @"Mr Burns."];
        return cell;
    }


That's it!! Now you can make a custom cell use a XIB file and screw those storyboard snobs!!!

[Grab the code!](https://github.com/MrMaksimize/iOS-Dev-Boilerplates/tree/master/ExampleCustomCell)

