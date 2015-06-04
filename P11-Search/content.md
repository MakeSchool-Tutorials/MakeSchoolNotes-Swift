---
title: "Search"
slug: search
---     

Let's take a look at adding a search bar to our `Dasboard Scene`.

[action]
See if you can add a `Search Bar` to your `Dashboard Scene` yourself.

![image](add_search_bar.png)

Much like our TableView we will need a `UISearchBar` `IBOutlet` to connect our search object to our controller code and we will be creating a new extension
to implement the search delegate `UISearchBarDelegate`.

We will then implement a state system into our `NoteViewController` for displaying notes normally (DefaultMode) and how to display when we are utilising search (SearchMode)

Let's add this outlet and display states.

[action]
Ensure the start of your `NotesViewController` looks as follows:

	class NotesViewController: UIViewController {
  
      @IBOutlet weak var searchBar: UISearchBar!
      @IBOutlet weak var tableView: UITableView!
  
      enum State {
        case DefaultMode
        case SearchMode
      }
      
      var state: State = .DefaultMode
      
[action]
1. Connect your Search Bar in your interface to the `searchBar` outlet.
2. Set the `Search Bar Delegate`, you can do this as you did before with `tableView` e.g. `searchBar.delegate = self` however
you can also do it by opening the *Connections Inspector* for the `Search Bar` object and dragging the delegate outlet to the `Dashboard`

![image](search_delegate_connect.png)

Let's add some search functionality, Realm can use `NSPredicate` to filter it's result set. `NSPredicate` allows you to construct logical conditions used to constrain a search.
It's easier to see it in action.

[action]
Add the following function to the `NotesViewController` class:

    func searchNotes(searchString: String) -> RLMResults {
      let searchPredicate = NSPredicate(format: "title CONTAINS[c] %@ OR content CONTAINS[c] %@", searchString, searchString)
      return Note.objectsWithPredicate(searchPredicate)
    }
    
This should be fairly clear to read, if the string in the search bar is found in either the title or content of a note, then return that note as part of the result set.

**Search Delegate*
Now we need our app to know when we are modifying our search bar, this is where the `UISearchBarDelegate` comes into play, add the follow extension to the `NotesViewController`

    extension NotesViewController: UISearchBarDelegate {
    
      func searchBarTextDidBeginEditing(searchBar: UISearchBar) {
        state = .SearchMode
      }
      
      func searchBarCancelButtonClicked(searchBar: UISearchBar) {
        state = .DefaultMode
      }
      
      func searchBar(searchBar: UISearchBar, textDidChange searchText: String) {
        notes = searchNotes(searchText)
      }
    
    }
    
Run your App, pretty nice eh? Although the search works well, however the experience could be better.  Let's tidy this up.
    
**State Manager**
 
When our `Dashboard` is presented we want to revert to `.DefaultMode`.
 
[action]
Ensure your `func viewWillAppear` now looks as follows:
 
    override func viewWillAppear(animated: Bool) {
      super.viewWillAppear(animated)
      state = .DefaultMode
    }
    
So we are setting the default state however nothing will happen unless we use the handy *didSet* functionality to perform actions when `state` is updated.

[action]
Ensure your `state` variable definition looks as follows:

    var state: State = .DefaultMode {
        didSet {
            // update notes and search bar whenever State changes
            switch (state) {
            case .DefaultMode:
                notes = Note.allObjects().sortedResultsUsingProperty("modificationDate", ascending: false) //1 
                self.navigationController!.setNavigationBarHidden(false, animated: true) //2
                searchBar.resignFirstResponder() // 3
                searchBar.text = "" 
                searchBar.showsCancelButton = false
            case .SearchMode:
                let searchText = searchBar?.text ?? ""
                searchBar.setShowsCancelButton(true, animated: true) //4
                notes = searchNotes(searchText) //5
                self.navigationController!.setNavigationBarHidden(true, animated: true) //6
            }
        }
    }
    
- 1) We have moved our default date search code so whenever we return to default state we want to ensure the list is reset.
- 2) This returns the navigation bar in an animated fashion, you can see why it was hidden in point 6
- 3) Remove keyboard popup
- 4) Animate in a cancel button beside the search bar (UI Polish)
- 5) Perform search on any text entered in search bar
- 6) This makes the search bar take prominance in our view, by hiding the navigation bar the user is focused on search. (UI Polish)

Run the App again, looking good!

![image](simulator_search.png)

Once again a good time to commit your code.

Well done you have made it this far and have a fully functionaly Notes application.  Sure it may be a little rough around the edges as you get to grips
with using Auto Layout and it may not be super pretty or colorful however it's your first App and a great starting place in your development :)

The next chapter is on polish, we will look at changing the color of various elements to put your own stamp on it.


