---
title: "Keyboard Input"
slug: keyboard-input
---     

In our app keyboard handling for the most part 'just works'. However the user experience is not optimal.
When you want to add a new note you would expect the keyboard to activate and set focus to the title field, if you viewing a note (with the option to edit) then you would
expect the note to be visible and only present the keyboard when a field is clicked on. 

How does the app know if we want to edit/create or view a new note? Well it doesn't of course, you will need to implement this.
You could create an `edit` variable that could be set in much the same way as the display note however I like to think about design
from a user experience perspective and how that relates to inferred behaviour.

If a Note has no title or content it stands to reason that you would want to edit it by default as an empty note is pretty pointless.
Let's add an `edit` variable flag and have it set when a `didSet` is called on our note.

[action]
Open `NoteDisplayViewController` and add a new `edit` boolean variable to this class and set it to a default value of `true`.
Now modify `func viewWillAppear` as follows:

	override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)
        
        displayNote(self.note)
    
        titleTextField.returnKeyType = .Next //1
        titleTextField.delegate = self       //2
        
        if edit {
            titleTextField.becomeFirstResponder() //3
        }
    }
	
- 1) We are renaming the 'Return' of the keyboard to 'Next', for our app it makes more sense from a user experience perspective that you most likely
want to move onto the next input field after entering text rather.  We will need to handle this in the `UITextFieldDelegate`.
- 2) Set the `titleTextField` delegate, we implement the delegate as a class extension as we did with our Table View Delegate.
- 3) If we are in 'Edit' mode then set the first responder, this will set focus to the titleTextField and bring up the keyboard ready for input. If we are not in edit
mode then the note will be displayed and no keyboard will popup until the user initiates this action themselves.

Time to set our edit flag based upon note content.

[action]
Modify `func displayNote` as follows:

    func displayNote(note: Note?) {
        if let note = note, titleTextField = titleTextField, contentTextView = contentTextView  {
            titleTextField.text = note.title
            contentTextView.text = note.content
            
            if count(note.title)>0 && count(note.content)>0 { //1
                edit = false
            }
        }
    }
    
 - 1) Check the length of our note content strings, if they have existing content then disable edit mode.
 
 We still need to add delegate for our textField.
 
 [action]
 Add the following extension:
 
    extension NoteDisplayViewController: UITextFieldDelegate {
    
        func textFieldShouldReturn(textField: UITextField) -> Bool {
            
            if (textField == titleTextField) {  //1
                contentTextView.becomeFirstResponder()
            }
            
            return false
        }
    }
    
 - 1) When the 'Return' button is pressed or in our case 'Next' this delegate will be called, we need to check that the `textField` in question is our `titleTextField` if so then
 move the user input onto editing the `contentTextView` leading to a nicer user experience.
 
 ![image](simulator_keyboard.png) 
 
 Great we can finally manage note content. 
 What do we do when the user starts to gather  a lot of notes?
 
 Adding some search functionality to our dashboard would be nice.  Let's tackle that in the next chapter....