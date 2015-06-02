---
title: "Capturing User Input"
slug: capturing-user-input
---     

The app is really coming along, it would be nice to start working on user interaction and enabling the user to create new Notes themselves.

1. Create View Controller Class
2. Add new View Controller in Storyboard
3. Rname 'New Note View Controller' / Assign Custom Class to our new controller subclass
4. Look at UINavigationItem in Dashboard, Rename Title in Atrributes.
5. add bar button item, modify identigier - add
6. Change to connections inspector then drag the triggered seques to the new note view controller, select present modally.
6a. You can of course dragg from 'Add' or even the '+' graphic however it's not always easy to select them and navigate depending on how busy your storyboard is.
7. what is a seque, what is a modal
8. Run it, nice! but we are stuck :)

9. Now there is no navigation controller so there is no navigation bar.  You can of course 'create' your own Navigation Bar by adding these objects however the preferred
method is to embed this view controller in a new navigation controller.
10. Add buttons, drag left cancel, drag right save.

11. Add @IBAction func unwindToSegue (segue : UIStoryboardSegue) {}

12. connect buttons to controller ext / unwindToSeque (add to noteviewcontroller)

13. move add code into prepareForSegue

14. label seques identifiers

15. save block

16. let's add the UITableViewDelegate  extension / add delegate

17. drag container view into new note view controller, will create a new view controller 

17. add manual seque, call it 'ShowExistingNote', drag from notes view controller to NoteDisplayViewController

18. add toolbar, set trash identifier

19. ctrl connect to exit, unwind seque, set identifier 'Delete'

20. create switch statement , save , delete