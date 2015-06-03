---
title: "New Note Controller"
slug: new-note-controller
---     

The app is really taking shape now, it would be nice to start working on user interaction and enabling the user to create new Notes themselves.

**Creating the New Note View Controller class*

Let's start by creating a new View Controller sublcass.

<div class="action"></div>
Select the `ViewControllers` group and then select *File/New/File* from the main menu in Xcode.

![image](new_controller_1.png)

Now select the *Cocoa Touch Class* and press `Next`

![image](new_controller_2.png)

Name your class `NewNoteViewController` ensure it is a subclass of `UIVewController` and you will of course be using `Swift`, press `Next`.

![image](new_controller_3.png)

Select your `ViewControllers` project folder and press `Create`.

![image](new_controller_4.png)

Your new `NewNoteViewController.swift` file has been created and added to the project.
Notice the code automatically created by Xcode for you, in particular the commented out section relating to seques, we will be coming back
to this powerful feature very soon.

Time to setup this controller in your storyboard.

**Creating a the New Note View Controller to your Storyboard*

If we add a new View Controller to the `Main.storyboard` it will be rather plain, it would be nice to have the *Navigation* Bar available, we can of course
build this by hand if we want however let's have Xcode do the work for us.

<div class="action"></div>
1. Open your `Main.storyboard` and drag in a `View Controller` from the object library.
2. Rename the View Controller to 'New Note View Controller' 
3. Assign the `Custom Class` to `NewNoteViewController` 

![image](new_controller_5.png)

Where is my Navigation? Xcode has your back.

<div class="action"></div>
With your `New Note View Controller` highlighted, select `Editor/Embed In/Navigation Controller`

![image](embed_in_navigation_1.png)

This will create a new `Navigation Controller` however don't worry we can get rid of it very shortly and still retain our Navigation.

![image](embed_in_navigation_2.png)

<div class="action"></div>
Select your `NotesViewController` select your `Navigation Item`, ensure you have the *Attributes Inspector* open and rename `Title` to Dashboard.

![image](navigation_item_1.png)

1. Find `Bar Button Item` in the *Objects Library*
2. Drag this to the top left of your `Dashboard` `Navigation Item`
3. Select the `Bar Button Item` and change `Identifier` to `Add`

![image](navigation_item_2.png)

Great, now how do we connect the `Add` button to the `New Note View Controller` ?  

Seque to the rescuse!

**Wht is a Seque**

A segue is a smooth transition. (Pronounced SEG-way)
Seques allow you to create transitions from one scene to another easily, you will be glad to know they are nice and easy to use.

Let's try one out right now and connect our '+' button to the `New Note View Controller`.

<div class="action"></div>
Select your `Add Bar Button Item` then Ctrl drag this to the `New Note View Controller`

![image](add_create_seque_1.png)

You will be presented with an additional dialog of seque types, for now we are going to use *Show*.  This will push the `New Note View Controller` to the top of
the Navigation stack.

![image](action_seque_1.png)

It's useful to add an *Identifier* to our seque, it comes in handy when you want to perform actions based upon the seque identifier, for example: Save, Add, Delete

Let's add an identifier to our new seque.

<div class="action"></div>
1. Select the seque, ensure the *Attributes Inspector* is select and then set the identifier to `Add`

![image](select_seque.png) ![image](seque_identifier_addseque_identifier_add.png)

Now that our `New Note View Controller` has been connected into our original Navigation Stack, we can remove the new one that was created during the embed in navigation
controller stage.

<div class="action"></div>
Remove the navigation controller that was added during the Embed stage. Feel free to move your controllers around your storyboard for optimal
asthetic pleasure :)

OK time to Run the App! Wooo Hoo, you can select Add and the app will now *Seque* into our New Note View Controller.

![image](screen_dashboard.png) ![image](screen_new_note.png)

**New Note Navigiation Options**

Let's add some traditional navigation options to our `New Note View Controller`, what actions would a user typically want to do?

- Cancel 
- Save

Well that's a good start.  Time for you to try out the following yourself.

<div class="action"></div>
1. Rename the `New Note View Controller` Navigation Item to `Add New Note`
2. Add a `Cancel` `Bar Button Item` on the left hand side of the bar. 
2. Add a `Save` `Bar Button Item` on the right hand side of the bar. 

<div class="hint"></div>
Make sure you set the button identifiers.

This should look as follows:

![image](new_note_bar.png) 

Awesome, you have some buttons ready but what can they be connected to? 
Well you could create some functions in the `New Note View Controler` however we are going to look at using *unwindToSegue* to help manage
our navigation stack, centralise our action functions and reduce code. 

**What is unwindToseque*

As the name suggests it will 'unwind' the current stack, so when our `New Note View Controller` was moves to the front after we pressed the + button. 
This will perform the opposite and return our root `Notes View Controller` to to the front.  
A seque will be used to transition between scenes and we can use the seque identifier to let us know which actions we need to perform.

Let's add this function and seque our buttons.

<div class="action"></div>
Open `NotesViewController.swift` and add the following to this class.

    @IBAction func unwindToSegue(segue: UIStoryboardSegue) {
        
        if let identifier = segue.identifier {
            println("Identifier \(identifier)")
        }
    }
	
Now connect both Cancel and Save bar buttons in `New Note View Controller` to the `Exit` Icon.  You will be presented with a popup to
select the `IBAction` to connect with.

![image](unwind_connection_baritems.png) 
![image](popup_unwindtoseque.png) 

You should now see to seques in the `Notes View Controller` outline.

![image](unwind_seque_selection.png) 

<div class="action"></div>
Select the first one, this will be the `Cancel` `Bar Item` connection, ensure the *Attributes Insepctor* is selected and give 
this an identifier of 'Cancel'. Modify the next seque to have an identifier of 'Save'.

Run your App!

Go in add 'Add' a new note, hit cancel, go back in and hit save.  Then take a look at your console output in the debug window.
You can see we now know which buttons are being pressed! The flow of the app is really coming on.

![image](debug_identifiers.png) 

When the user hits `Cancel` we don't really need to do anything, however when they `Save` we want to add a new Note.  Before we tackle user
input lets ensure our process to save works.

First of all we are going to create a new Note in our `NewNoteViewController`, will will do this in our `prepareForSeque` function, this was auto-generated by Xcode and commented out.

<div class="action"></div>
Open `NewNoteViewController.swift` and perform the following actions:
1. Add a variable to the class to hold our new Note
2. Uncomment the `prepareForSeque` function and setup a dummy Note with a little comment. (Look at `viewDidLoad` in `NotesViewController` for a hint)

<div class="solution" title="Solution"></div>
Solution #1
    class NewNoteViewController: UIViewController {   
        var currentNote: Note?

Solution #2
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        // Get the new view controller using segue.destinationViewController.
        // Pass the selected object to the new view controller.
        
        currentNote = Note()
        currentNote!.title   = "Super Simple New Note"
        currentNote!.content = "Yet More Content"
    }

Great whenever you navigation to `Add New Note` a new note will be created however once you exit this controller the note will be lost and forgotten about.
We need something to grab this Note data and save it when the user presses the `Save` button.  Well we've already seen we are alerted through our `unwindSeque` when the `Add` action
is performed. So let's look there.

<div class="action"></div>
Open `NotesViewController.swift` and look at your recently added `unwindToSeque` function.  Time to expand this, modify your code as follows:

    if let identifier = segue.identifier {
            let realm = RLMRealm.defaultRealm()
            
            switch identifier {
            case "Save":
                let source = segue.sourceViewController as! NewNoteViewController //1
                
                realm.transactionWithBlock() {
                    realm.addObject(source.currentNote)
                }

            default:
                println("No one loves \(identifier)")
            }
        }
        
        notes = Note.allObjects().sortedResultsUsingProperty("modificationDate", ascending: false) //2
        
You are now using a switch statement, for one case you would most likely use an `if` statement however we will be expanding this `switch` statement with additional use cases.
As it stands we have just added suporrt for our `Save Action`.
1. We need to grab a reference to the outgoing controller, in this case our `New Note View Controller`, we do this to gain access to the `currentNote` variable that holds the new Note object.
2. Realm allows for advanced sorting and query functionality for it's stored objets, preivously we just grabbed all Note objects without any regard for order, this change makes it more useful 
and orders by the most recent `modificationDate`.

Before you run the app let's tidy up the `viewDidLoad()` function, previously you added test code to create a new Note everytime the app is run.  Time to tidy this code up now.

<div class="action"></div>
Ensure your `viewDidLoad()` function reads as follows:

    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.dataSource = self
        
        notes = Note.allObjects().sortedResultsUsingProperty("modificationDate", ascending: false)
    }
    
Run the App! You will notice it's still filled with all the previously added Notes, time to clear this out. 

<div class="action"></div>
With the simulator in focus, select `iOS Simulator\Reset Content and Settings...` Then stop the App.

Run the App again! This time your Table View should be empty. Select `Add` and then `Save`.

Woo hoo, the app should now return to the Dashboard and you will see the note has been added. Good work.

**Adding the Table View Delegate**

The Table View Delegate was touched upon in the *Introduction To Table Views* chapter, we didn't implement it at the time as it wasn't required at that point however now
would be a great time to add an `Extenstion` to the `Notes View Controller` to implement this delegate so we can handle editing of an existing row or deletion of a row.

<div class="action"></div>
Ensure you have `NotesViewController` open and add the following code to the end of your file:

    extension NotesViewController: UITableViewDelegate {
    
        func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
            //selectedNote = notes.objectAtIndex(UInt(indexPath.row)) as? Note      //1
            //self.performSegueWithIdentifier("ShowExistingNote", sender: self)     //2
        }
        
        // 3
        func tableView(tableView: UITableView, canEditRowAtIndexPath indexPath: NSIndexPath) -> Bool {
            return true
        }
        
        // 4
        func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
            if (editingStyle == .Delete) {
                let note = notes[UInt(indexPath.row)] as! RLMObject
                
                let realm = RLMRealm.defaultRealm()
                
                realm.transactionWithBlock() {
                    realm.deleteObject(note)
                }
                
                notes = Note.allObjects().sortedResultsUsingProperty("modificationDate", ascending: false)
            }
        }

    }

So what is going on? Alt click on a function to quickly get an overview of what these new function do.

The first function informs us that a row has been selected, you will notice these lines have been commented out.

1. When a note has been selected we want to assign this note to a variable for easy access. When a row is selected the row Index is passed as a parameter so
we can grab the correct note object using the `objectAtIndex` method to return the correct note object.

2. We well be performing a seque to a new Note Display View Controller (You will add this soon) that will display the `currentNote`

<div class="action"></div>
Can you add a `currentNote` variable to store the selected Note? (Hint Hint), once you have uncomment the first line so the `selectedNote` can be assigned.

Before you setup the Note Display View Controller, let's look at 3 and 4.

3. This function is used to check if a row can be editied, in our app this will always return true.

4. This function is activated when you left swipe your Table View to enter edit mode and are presented with the option to *Delete* the selected row.

If you now run the App, it will not do any of this :(  This is because we need tell the Table View where it can find the delegate methods.

<div class="action"></div>
See if you can add the `delegate` yourself, it's very similar to setting the `dataSource` and can come straight after this code.

<div class="solution></div>
Your `viewDidLoad` function should look as follows:

    tableView.dataSource = self
    tableView.delegate = self
    
    notes = Note.allObjects().sortedResultsUsingProperty("modificationDate", ascending: false)
    
Run the app! Give it a left swipe, oh no, it swipes left but I can't see the `Delete` button :(

We need to go back and add some contraints to ensure the Table View fits our device view.

<div class="action"></div>
Open up your `Dashboard Scene` in `Main.storyboard` and select the `Table View` then select `Pin` and add the following constraints.

![image](constraint_table_view.png) 

Now run the App, the rows will fit nicely and a left swipe will now reveal the `Delete` button, go on press it...

![image](simulator_delete.png) 

Great your Notes app has progressed nicely, you can now perform note management actions and have implemented the Table View delegate.  

Time to move on and create a new controller to display the contents of a note and allow us to modify the contents.

17. drag container view into new note view controller, will create a new view controller 

17. add manual seque, call it 'ShowExistingNote', drag from notes view controller to NoteDisplayViewController

18. add toolbar, set trash identifier

19. ctrl connect to exit, unwind seque, set identifier 'Delete'

20. create switch statement , save , delete