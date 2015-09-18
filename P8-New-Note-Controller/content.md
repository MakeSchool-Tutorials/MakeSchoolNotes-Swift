---
title: "New Note Controller"
slug: new-note-controller
---

The app is really taking shape now! It would be nice to start working on user interaction and enabling the user to create new notes themselves.

#Creating the New Note View Controller class

Let's start by creating a new view controller subclass.

> [action]
> Select the *ViewControllers* group and then select *File/New/File* from the top toolbar in Xcode. Then, select the *Cocoa Touch Class* (under iOS, not OS X!) and press *Next*.
>
> ![image](new_controller_1.png)
>
> Name your class "NewNoteViewController". Make it a subclass of `UIViewController` - and you will of course be using Swift. Press *Next*.
>
> ![image](new_controller_2.png)
>
> Select your *ViewControllers* project folder and press *Create*.
>
> ![image](new_controller_3.png)
>
> The new `NewNoteViewController.swift` file has been created and added to the project.
>
> ![image](new_controller_4.png)

Notice the code automatically added to the file by Xcode - in particular the commented out section relating to segues. We will be coming back
to this very powerful functionality soon.

#Creating the new Note View Controller interface

You should continue reading through the next few steps, to better understand exactly what's going on, but if you get stuck you can refer to this video:

<video width="100%" controls>
  <source src="https://s3.amazonaws.com/mgwu-misc/SA2015/AddViewControllerAndSetUpNavigation.mp4" type="video/mp4">

Now let's connect the new view controller to `Main.storyboard` so users can create their own notes.

> [action]
> 1. Open `Main.storyboard` and drag in a `View Controller` from the object library.  
> 2. Assign *Custom Class* identifier to *NewNoteViewController* so it'll use the Swift file we just created above. This should also change this view controller's name to "New Note View Controller" in the *Document Outline*.
>
> ![image](new_controller_5.png)

#Navigation

Now let's give our navigation bar on the *NotesViewController* a title and insert an add button to allow users to create a new note. When this button is pressed the app will navigate to our *NewNoteViewController*.

> [action]
> Select *NotesViewController* in Interface Builder, then select the `Navigation Item`, ensure you have the *Attributes Inspector* open, and change the name in the *Title* field to "Dashboard".

>
> ![image](navigation_item_1.png)
>
> 1. Find `Bar Button Item` in the *Objects Library*.
> 2. Drag this new bar button item to the top left of the navigation item in your Dashboard.
> 3. Select this newly created bar button item and change the *Identifier* to *Add*.
>
> ![image](navigation_item_2.png)

Great! Now how do we connect the *Add* button to the *NewNoteViewController*?  

Segue to the rescue!

#Segues

A segue is a smooth transition. (Pronounced SEG-way, to avoid that awkward interview moment.) Segues allow you to easily create transitions from one view controller to another. You will be happy to know they are nice and easy to use.

Let's try one out right now and connect our '+' button to the *NewNoteViewController*.

> [action]
> Select the *Add* Bar Button Item then *Ctrl-Drag* this to the *NewNoteViewController*.
>
> ![image](add_create_segue_1.png)

You will be presented with an additional dialog of segue types: for now we are going to use *Show*.  This will push the *NewNoteViewController* to the top of the navigation stack.

![image](action_segue_1.png)

It's useful to add an *Identifier* to our segue. It comes in handy when you want to perform actions based upon the segue identifier, like Save, Add, or Delete.

Let's add an identifier to our new segue.

> [action]
> 1. Select the segue.
> 2. Open the *Attributes Inspector* and set the segue's identifier to *Add*
>
> ![image](add_segue_identifier.png)
>

Feel free to move your controllers around your storyboard so everything lines up just how you like it :)

OK, time to Run the App!

Wooo Hoo! You can select the + and the app will now *segue* into our new note view controller.

![image](screen_dashboard.png) ![image](screen_new_note.png)

#New Note Navigation Options

Let's add some traditional navigation options to our *NewNoteViewController*. What actions would a user typically want to do?
Well....

- Cancel
- Save

Those look like a good start.  See if you can implement the following by yourself:

> [action]
>
> 1. Add a `Navigation Item` from the Object library to the *NewNoteViewController* in Interface Builder
> 2. Change the name of the *NewNoteViewController* in Interface Builder "Add New Note"
> 3. Add a *Cancel* `Bar Button Item` on the left hand side of the bar.
> 4. Add a *Save* `Bar Button Item` on the right hand side of the bar.

Here's a possible solution:

> [solution]
> You need to set the button identifiers.
>
This should look as follows:
>
> ![image](new_note_bar.png)
>

Awesome! You have some buttons ready. But what should they be connected to?

Well, you could create some new methods for each action in the *NewNoteViewController*. However, we are going to look at using *unwindToSegue* to
help manage our navigation stack, centralize our action functions, and reduce code.

#What is unwindToSegue

As the name suggests, it will 'unwind' the current stack. Remember when our *NewNoteViewController* was moved to the front after we pressed the + button? This will perform the opposite and return our root *NotesViewController* to the front. A segue will be used to transition between scenes. We can use the segue identifier to let us know which actions we need to perform.

Let's add this function and segue our new bar button items.

> [action]
> Open `NotesViewController.swift` and add the following function to the class.
>
    @IBAction func unwindToSegue(segue: UIStoryboardSegue) {
>
        if let identifier = segue.identifier {
            print("Identifier \(identifier)")
        }
    }
>
>
Now drag both the *Cancel* and *Save* bar buttons in *NewNoteViewController* to the *Exit* Icon.  You will be presented with a popup to select the `IBAction` to connect to.
>
> ![image](unwind_connection_baritems.png)
> ![image](popup_unwindtosegue.png)

You should now see the segues in the `Notes View Controller` outline.

![image](unwind_segue_selection.png)

> [action]
> Select the first segue in the list. This will be the *Cancel* `Bar Button Item` connection.
> Open the *Attributes Inspector* and set the identifier to 'Cancel'.
> Select the next segue in the list and give it an identifier of 'Save'.

Run your App!

Go ahead and click the *Add* button to add a new note. Then hit *cancel*. Click add again, then hit *save.*  Then take a look at your console output in the debug window.
You can see we now know which buttons are being pressed! It's good to get a feel for the flow of your app.

![image](debug_identifiers.png)

When the user hits *Cancel* we don't really need to do anything. However, when they *Save*, we want to add a new Note.  Before we tackle user
input, let's ensure our process to save works.

#Creating Data

First of all, we are going to create a new note in our *NewNoteViewController*. We will do this in our `prepareForSegue` function. This code block was auto-generated by Xcode and commented out.

> [action]
> Open `NewNoteViewController.swift`.
> 1. Add a variable to the class to hold our new Note.
> 2. Uncomment the `prepareForSegue` function and set up a dummy Note with a little bit of content.
> Hint: Look at `viewDidLoad` in *NotesViewController* to see this process.

Here's a possible solution:

> [solution]
> Adding a note variable:
>
    class NewNoteViewController: UIViewController {
        var currentNote: Note?
>
> Creating a new note and populating with dummy content:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        // Get the new view controller using segue.destinationViewController.
        // Pass the selected object to the new view controller.
>
        currentNote = Note()
        currentNote!.title   = "Super Simple New Note"
        currentNote!.content = "Yet More Content"
    }
>

#Saving Data

Great. Now whenever you navigate to *Add New Note*, a new note will be created. However, once you exit this controller the note will be lost and forgotten about. We need something to grab this note data and save it when the user presses the *Save* button.
We've already seen that we are alerted through our `unwindSegue` when the *Add* action is performed. So let's look there.

> [action]
> Open `NotesViewController.swift` and locate the `unwindToSegue` function.  Modify your code as follows:
>
if let identifier = segue.identifier {
>  
      do {
        let realm = try Realm()
>    
        switch identifier {
>    
        case "Save":
          // 1
          let source = segue.sourceViewController as! NewNoteViewController
          try realm.write() {
            realm.add(source.currentNote!)
          }
>    
        default:
          print("No one loves \(identifier)")
>    
        }
>    
        // 2
        notes = realm.objects(Note).sorted("modificationDate", ascending: false)
      } catch {
        print("handle error")
      }
    }

You are using a switch statement here, and I know what you're thinking. Normally for only one case, you would use an `if` statement, right? However, we will be expanding this `switch` statement with additional use cases.
As it stands, we have just added support for our *Save Action*.

Take a look at the numbered comments in the code. Here's what they mean:
1. We need to grab a reference to the outgoing controller, which in this case is our *NewNoteViewController*. We do this to gain access to the `currentNote` variable that holds the new Note object.
2. Realm allows for advanced sorting and query functionality for its stored objects. Previously, we just grabbed all note objects without any regard for order. This change makes the app more useful and orders by the most recent `modificationDate`.

Before you run the app, let's tidy up the `viewDidLoad()` function in *NotesViewController*. Previously, you added test code to create a new Note every time the app is run.  Time to tidy this code up now.

> [action]
> Modify your `viewDidLoad()` method to read as follows:
>
      override func viewDidLoad() {
        super.viewDidLoad()
        tableView.dataSource = self
>
        do {
          let realm = try Realm()
          notes = realm.objects(Note).sorted("modificationDate", ascending: false)
        } catch {
          print("handle error")
        }
      }
>

Run the app! You will notice it's still filled with all the previously added notes - time to reset everything.

> [action]
> With the simulator in focus, select *iOS Simulator\Reset Content and Settings...* then quit the simulator.

Run the app again! This time your table view should be empty.

> [action]
> Select *Add* and then *Save*.

Woo hoo, the app should now return to the Dashboard and you will see the note has been added. Good work.

#Adding the Table View Delegate

We touched upon the table view delegate in the *Introduction To Table Views* chapter, but we didn't implement it at the time as it wasn't required at that point. However, now would be a great time to add an `extension` to the *NotesViewController* to implement this delegate so we can handle editing of an existing row or deletion of a row.

> [action]
> Open `NotesViewController.swift` and add the following code to the end of your file:
>
extension NotesViewController {
>
      override func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        //1
        //selectedNote = notes[indexPath.row]    
>
        // 2
        //self.performSegueWithIdentifier("ShowExistingNote", sender: self)    
      }
>      
      // 3
      override func tableView(tableView: UITableView, canEditRowAtIndexPath indexPath: NSIndexPath) -> Bool {
        return true
      }
>      
      // 4
      override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
        if editingStyle == .Delete {
          let note = notes[indexPath.row] as Object
>          
          do {
            let realm = try Realm()
            try realm.write() {
              realm.delete(note)
            }
>           
            notes = realm.objects(Note).sorted("modificationDate", ascending: false)
          } catch {
            print("handle error")
          }
        }
      }
>      
    }

So what is going on here? Remember you can Alt-Click on a function to quickly get an overview of what it does.

The first function informs us that a row has been selected. You will notice these lines have been commented out.

*Comment Review*

1. When a note has been selected, we want to assign this note to a variable for easy access. When a row is selected, the row index is passed as a parameter so we can grab the correct note object using the subscript syntax.

2. We will be performing a segue to a *NoteDisplayViewController* (you will add this soon) that will display the selected note.

> [action]
> Can you add a `selectedNote` variable to the class to store the selected Note?
> **Hint** you need to uncomment the first commented line so the `selectedNote` can be assigned.

Before you set up the Note Display View Controller, let's look at 3 and 4.

3. This function is used to check if a row can be edited. In our app we would always like this behavior, so it will always return true.
4. This function is activated when you left swipe your table view to enter edit mode and are presented with the option to *Delete* the selected row.

#Setting The Delegate

Finally, lets set the delegate.

> [action]
> See if you can add the `delegate` yourself. It's very similar to setting the `dataSource` and can come straight after this code.

Here's the solution:

> [solution]
> Add the new line below where you assigned the `dataSource` in the `viewDidLoad` function:
>
    tableView.dataSource = self
    tableView.delegate = self

Great! Your Notes app has progressed nicely. You can now perform note management actions and have implemented the table view delegate.  

Time to move on and create a new controller to display the contents of a note and allow us to modify the contents.
