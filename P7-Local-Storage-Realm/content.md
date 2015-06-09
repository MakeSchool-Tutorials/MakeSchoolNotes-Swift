---
title: "Local Storage with Realm"
slug: local-storage-realm
---     

We have taken first steps of building a Notes App together, you can now create your own custom table view listing and populate it with some basic test data.

In most Apps you will want a way to archive your user data, having a list is great however the user is going to be pretty unimpressed if your app loses all
their precious notes after the app is restarted.  That's a 1 Star review waiting to happen.

##Persistence

Persistence is the ability of an object to survive the lifetime of the OS process in which it resides.

**How to achieve persistence**

- Should be transparent to application developer
- Upon activation, load object state from persistent storage
- Storing object state on persistent storage before de-activation

Hold up... This sounds like it could be painful...  

Thankfully this is an age old problem with many differnent solutions, Apple offers you [Core Data](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreData/Articles/cdTechnologyOverview.html) as a complete framework for object graph management.
It can be a great solution however it is also a lot of work :)

Let's look at a lightweight alternative called Realm. 

##Realm

What is [Realm](https://realm.io/)?

*Realm is a replacement for SQLite & Core Data. It can save you thousands of lines of code & weeks of work, and lets you craft amazing new user experiences.*

Hmmmmm... I like saving lines of code and I like saving work... I'm sure Realm says that to all the boys/girls...

Seriously let's take it for a spin. Time to implement Realm into our `Note` object, you can find this under the `Entities` group.

    import Foundation
    import Realm

    class Note : RLMObject { 
    }

So we've dynamically imported the Realm library so we have access to this functionality.  You remeber Alt-Clicking? Try it now on *RLMObject*.
Realm objects are just like normal object, you just subclass *RLMObject* to get started.

> [action]
> Let's add the following variables to create our `Note` model class.
>
    dynamic var title: String = ""
    dynamic var content: String = ""
    dynamic var modificationDate = NSDate()
>

##Dynamic Attribute

What is all this **Dynamic** business?
This attribute informs the Swift compiler that storage and implementation of a property will be provided at runtime.

You will be pleased to know that's all it takes to implement your `Note` data model, nice!

##Displaying Realm Object Data

Time to knuckle down, it's going to take a little bit of coding to switch over to Realm and add a test note in code.

> [action]
> Open `NoteTableViewCell.swift` and enter the following code:
>
    class NoteTableViewCell: UITableViewCell {
>    
        // initialize the date formatter only once, using a static computed property
        static var dateFormatter: NSDateFormatter = {
            var formatter = NSDateFormatter()
            formatter.dateFormat = "yyyy-MM-dd"
            return formatter
            }()
>        
        @IBOutlet weak var titleLabel: UILabel!
        @IBOutlet weak var dateLabel: UILabel!
>        
        var note: Note? {
            didSet {
                if let note = note, titleLabel = titleLabel, dateLabel = dateLabel {
                    self.titleLabel.text = note.title
                    self.dateLabel.text = NoteTableViewCell.dateFormatter.stringFromDate(note.modificationDate)
                }
            }
        }
>            
    }
>

##Code Optimisation
Wow what is with `static var dateFormatter` well glad you asked, this is a code optimisation for a `NSDateFormatter` object, some objects are nortiously slow to initialize so you want to be able to reuse them instead.
When I say 'slow' this is a very relative term however if you are processing hundreds of objects, it all adds up and if it only takes a few lines of code to optimise then it's time well spent.

I wouldn't expect you at this stage to start worrying about optimisations, focus on your application experience first.  However it's good to know there is always a little extra juice 
that can be squeezed out of an app, this comes with experience.

##didSet

So you've added a variable to store the `Note` object, what is didSet? Well it's a rather handy bit of functionality that will be called whenever this `note` object is modified. 
For example if it gets edited anwyhere, this function will be called that will updated the OutLet labels and therefore update the `NoteCell` in our list.

##Notes Collection

Before you create a new note, you need to ensure you add a notes variable to our `NotesViewController` so we can populate the Table View.

> [action]
> Add the following code to after your `tableView` variable in `NoteViewController`
>
    var notes: RLMResults! {
        didSet {
            // Whenever notes update, update the table view
            tableView?.reloadData()
        }
    }
>

Once again notice the user of *didSet* to refresh the tableView when Notes results are updated, very handy. Let's add some notes.

##Creating A New Note

Our `NoteCell` can now display information from a `Note` object, let's create one to see this in action.  

> [action]
> Add the following to the end of your *viewDidLoad* function in `NotesViewController.swift` to initialize a new `Note` object.
>
    let myNote = Note()
    myNote.title   = "Super Simple Test Note"
    myNote.content = "A long piece of content"
>

Great you have a new note but nowhere to put it, let's add it to our `Realm` local storage with a simple 3 Step Process.

> [action]
> Add the following code right after the previous code.
>
    let realm = RLMRealm.defaultRealm() // 1
    realm.transactionWithBlock() { // 2
        realm.addObject(myNote) // 3
    }
>
> 1. Before you can add it to Realm you must first grab the default realm.
> 2. Write operations must be performed within a real transaction.
> 3. Add your new note to realm

Realm makes this whole process nice and easy.

> [action]
> Finally before the closing squiggley of `viewDidLoad()` let's update our `notes` variable with our latest Realm data.
>
    notes = Note.allObjects()
>

Very close now.....

Remeber when you added the `UITableViewDataSource` protocol extension? These functions now need updated to pull through the data from your new notes data source.

> [action]
> Replace the following code in `tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath)`
>
    let row = indexPath.row
    cell.textLabel?.text = "Hello World"
>
> with
> 
    let row = UInt(indexPath.row)
    let note = notes[row] as! Note
    cell.note = note
>
> Also replace the following code in `func tableView(tableView: UITableView, numberOfRowsInSection section: Int)`
>
    return 5
>
> with
>
    return Int(notes?.count ?? 0)
>

Finally, it's time to Run the App!

It should look a little like this:

![image](notes_app_realm.png)

The more times you run it, the more notes will be added.  
If you wish to clear out the notes for testing, add the following into your realm transaction block. `realm.deleteAllObjects()`

As you have found out, Realm is a great lightweight framework to add data persistance to your app.  
You also explored how to add new notes in code, the app is starting to come together now.

Now would be a great time to **Commit** your work.

In the next chapter we will be looking at the next steps to enable the capture of user input to create new notes.
