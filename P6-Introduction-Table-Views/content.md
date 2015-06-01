---
title: "Introduction Table Views"
slug: introduction-table-views
---     

Table Views are frequently found in iOS apps. I'm sure you will have seen them many times.

Let's take a sneak peek of MakeSchoolNotes a little bit further on in the tutorial :)

![image](app_preview_1.png)

A Table view is an instance of the `UITableView` class, it has only one column and only allows vertical scrolling.  
Rows are drawn using cells which are `UITableViewCell` objects.  

As you can see it's an ideal way to display lists of information, perfect for our app.

<div class="action"></div>
Open `Main.Storyboard` you will see that you have two `Controllers`.

On the right hand side you can see the *Table View* inside the `View Controller`.

![image](viewcontroller_table_view.png)

Ensure you have expanded *Document Outline*: 

![image](tableview_outline.png)

You can inspect see the object hierarchy for our main `View Controller` which contains the following objects of interest:

`View`
 - `Table View`
   - `Table View Cell`
     - `Content View`
     
I always encourage developers to look at new objects under the various *Inspector* tabs.

You may have noticed if you click on the `View Controller` and inspect under the *Identity Inspector* it is using a custom class of **NotesViewController**, you will be expanding
on this class very soon.
 
We will come back to this very shortly however let's look at the other `Controller` in our Storyboard.
 
##Navigation Controller
 
What is a *Navigation Controller*? Well there is a big clue in the name :)

The *Navigation Controller* manages a stack of view controllers, it provides a drill-down interface for hierarchical content. It often goes hand in hand with Table Views, for example
look at the *Photos* App. Click `Albums` to take you into a View Controller that presents the UITableView, click a row and it this opens a View Controller to display image thumbnails, drill down again and you go into another View Controller that will preview the image.

Notice when you are *Navigating* the app you will always have at the top of your app a `Navigation Bar` provided by the `Navigation Controller` that sits at the root of your app, this enables 
you to perform actions such as *Back* that help manage your stack of view controllers.

 
##Delegates & Data Sources

You have a Table View however it's not much use without any content, so let's get started.  You want to be able to access your Table View progrmatically so you will need to create
a *IBOutlet* to connect your Table View from Interface Builder to your code.  A sensible place to manage this would be in your *NotesViewController*.

<div class="action"></div>
Let's see if you can add an oulet variable to the NotesViewController, I'll give you a clue the object type is *UITableView*.

<div class="solution" title="Adding The Table View Outlet"></div>
It should look something like this:

```
class NotesViewController: UIViewController {
    
    @IBOutlet weak var tableView: UITableView!
```

If you did it without looking, give yourself a smile.

Great you now have a place to connect your Table View.

<div class="action"></div>
1. Go back into *Main.Storyboard* and select the `Table View` inside the `Home Scene`. 
2. With this selected click on the *Connections Inspector* and look at `Referencing Outlets`
3. Create a `New Referencing Outlet` to your ViewController (Home with the yellow circle just below 'Home Scene')

When you let go, you should notice that you will be presented with two options.

![image](code_connect_tableview_2.png)

So you have the option of connecting to either the View or the new tableView variable you just created.
Select the tableView outlet.

Let's change the View Controller name while we are at it.

<div class="action"></div>
Click on *Home* and then press *Enter* and rename to `Dashboard`

Great your Table View is now connected to tabelView variable in `NotesViewController`. 
So Hit run!

Oh it looks exactly the same, well you havn't added any data and before it can display anything the tableView needs a data source and also requires delegate functions
to be setup before it will run.

**Data Source**

This one is a bit more self explanatory, for the Table View to display data it requires a data source to populate from. This might be an array or information, it could be data being queried
from a local database, it might be data being pulled down from a remote source.

Let's tell our tableView where it should expect to find a dataSource handler, let's try setting this one via code than via Interface Builder.

<div class="action"></div>
1. Open `NotesViewController` and locate the `viewDidLoad` function.
2. Add the following code to this function:

```
    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.dataSource = self
    }
```

You will notice a red exclamation mark, you just made Swift sad.
./MakeSchool Tutorial Content/MakeSchoolNotes/MakeSchoolNotes/ViewControllers/NotesViewController.swift:18:32: Cannot assign a value of type 'NotesViewController' to a value of type 'UITableViewDataSource?'

Well this is very true, as it stands in our code we have not added any support for the `UITableViewDataSource` protocol in our `NotesViewController`.

**What is a protocol?**

Good question, a protocol defines a blueprint of methods, properties and other requirements that suit a particular task or piece of functionality.  A protocol
will not implement any code for you, it only describes what the implemenation will look like. The input it will take and the output it expects from your implementation.

OK great so how do I add this protocol support for `UITableViewDataSource`?

**Extensions**

Glad you asked, you can extend support to your existing class an *Extension*. Extensions can add new functionality however they can't override existing functionality. 
In this case you will be extending your class to implement the additional protocol functionality.

Before we add this code 

<div class="action"></div>
Add the following snippet after the closing squiggly bracket on you `NotesViewController` class definition. Your class should now look as follows:

```
extension NotesViewController: UITableViewDataSource {
    
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCellWithIdentifier("NoteCell", forIndexPath: indexPath) as! NoteTableViewCell

        let row = indexPath.row
        cell.textLabel?.text = "Hello World"
        
        return cell
    }
    
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 5
    }
    
}
```

Don't run it just yet, Swift is still a bit sad.

A handy hint to find out more information for any function is to Alt-Click to see a description from the Apple Library Documentation. 
Try it out now on your newly added `func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int` function. 

![image](table_view_protocol_lookup.png)

If you look at the first line in the first function which is used to populate the TableViewCells with information. 

```
let cell = tableView.dequeueReusableCellWithIdentifier("NoteCell", forIndexPath: indexPath) as! NoteTableViewCell
```

This code is trying to return a `UITableViewCell` with a unique identifier of "NoteCell" (You can create custom Table View Cells so you could have many unique styles in your application).
*dequeueReusableCellWithIdentifier* is trying to make this process more efficent, try and find a reusable cell first and save on additional initializing overhead.  
If none exist, which will be the case when we run our app the it will create new `UITableViewCells` and in our case we are going to use our own custom subclass of `NoteTableViewCell`.

Have a look at the code in `NoteTabelViewCell` right now it doesn't do anything more than a standard `UITableViewCell`, as I'm sure you've guessed, you will be adding to this soon.

Let's quickly fix our `Table View Cell`.

<div class="action"></div>
1. Ensure you are in `Main.storyboard`
2. Select your `Table View Cell`
3. Using the *Identity Inspector* set your *Custom Class* to `NoteTableViewCell`
4. Using the *Attributes Inspector* set your *Style* to `Basic` and *Identifier* to `NoteCell`
5. Let's also rename the `Table View Cell` in the *Document Outline* hierarchy to `NoteCell`

![image](table_view_cell_identity_inspector.png) ![image](table_view_attributes_inspector.png)

OK let's hit Run, by the power of grey skull, you should now see the following:

![image](table_view_hello_world.png)

**Commit**
Now would be a nice time to commit your code, commit your code and add a message to remind you what you've implemented.

Great, so you can now display *Hello World* 5 times on the screen.  The second function in your `UITableViewDataSource` extension returns the number of rows to be populated by the data source.
In this case it's been hardcoded to 5 simply for testing, typically it will be the count of an array of objects.

If you click on a row, you will notice it will be `highlighted` however it will not do anything else.  This is where the `UITableViewDelegate` protocol comes into play, this protocol
contains the optional methods to allow you to interact with your rows, the most common method implemented being when you click on the row to take the user to another View Controller to display
more information.

Before we tackle this let's pimp our Table View Cell.

##Custom Table View Cells

Now is a good time to think about our application design, so we have the ability to display a list of information however what information should we store?
A good starting point would be:

- Title
- Modification Date
- Content

Right now our basic cell only contains a title, you may have noticed a few styles under the `Table View Cell / Style` dropdown, you are going to roll your own and create a custom cell.

<div class="action"></div>
Select your `NoteCell` in the Interface Builder and expand the hierarchy.

It should look like this:

![image](notecell_custom_1.png)

Let's see if you can create a custom cell with a title label and modification date label, increase the cell height if neeeded and feel free to add plausable placeholder text in your labels.  
It makes it much easier to visualise and ensure you have the right asthetic.

*Hint*: Make sure you change the `Style` to `Custom` first, this will give you an empty `Content View` to work with.

Hopefully yours will look something like this, ideally with a bit more swag than mine.

![image](notecell_custom_2.png)

Great, now you want to ensure you can access the label objects from your `NoteTableViewCell` object.  
Let's see if you can do this one on your own, you will want to create two Outlet variables of type `UILabel` in `NoteTableViewCell.swift` and then 
connect the `Custom Cell View` label objects respectivley in IB.

<div class="solution" title="Adding Cell Outlets"></div>
Open `NoteTableViewCell.swift` and add the following.

```
class NoteTableViewCell: UITableViewCell {
    
    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var dateLabel: UILabel!
    
}
```

Your outlets are now ready.

<div class="solution" title="Connecting Outlets"></div>
Open `Main.storyboard`, select your `NoteCell` then select *Connections Inspector*, you will see `dateLabel` and `titleLabel` listed as `Outlets`.
Drag the Outlets to your Title Label and Date Label objects in your `Prototype Cells` view. It should now look like this:

![image](notecell_custom_connection.png)

Let's quick see this in action, return to your `NotesViewController` and modify the function that populates your `UITableView` to use these new labels.

<div class="solution" title="Applying Custom Cell Data"></div>
1. Hopefully you spotted the previous line of code `cell.textLabel?.text = "Hello World"` and replace with the following:

```
cell.titleLabel.text = "Hello";
cell.dateLabel.text  = "Today";
```

Run your App.

![image](app_custom_cell_1.png)

Pretty sweet.  You have explored some fundamental skills and this is certainly not the last time you will hear of `Protocols`, `Delegates` and `Extensions`.

Adding hardcoded data to populate you TableView is interesting and demonstrates the basics however it's not that useful.  Time to move on now and see how
local storage using Realm can make this App a lot more useful.