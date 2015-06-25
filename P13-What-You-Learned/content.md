---
title: "What you have learned"
slug: what-you-learned
---

Congrats on finishing *MakeSchoolNotes* and building your first iPhone app!

At this point you have been introduced to many concepts in iOS app development. These concepts form the building blocks of iOS app development and you will expand upon this knowledge in the next tutorial.

Let's take a look at what you have learned so far.

###Controllers & Views

* **View Controllers**: These are the cornerstones of designing your application flow and *user screens*.

* **Navigation Controllers**: The Navigation Controller manages a stack of view controllers that generally flow from left to right (and vice versa). It provides a drill-down interface for hierarchical content, and it often goes hand-in-hand with Table Views.

* **Table Views**: The table view allows us to display and edit hierarchical lists of information. (Perfect for a notes app.) This is an essential view and one you will see often in application design.

* **Table View Cells**: You have seen how easy it is to customize table view cells; although ours was a simple design, you could easily expand with additional information.

* **Container Views**: We used a container view to provide a common view controller interface for viewing, creating, and editing notes.

###Delegates, Protocols, & Extensions

* **Delegates**: A delegate refers to the **Delegation Design Patten**. Using this design pattern, a class would have certain operations that it delegates out (perhaps optionally). Doing so creates an alternative to subclassing by allowing specific tasks to be handled in an application-specific manner, which would be implemented by a delegate. 

* **Protocols**: A protocol is a set of method declarations and properties (an interface) that a type can choose to implement (called *conforming to a protocol*). When a class conforms to a protocol, it implements all of the protocol's **required** methods and properties, and may also implement all or some of the protocols **optional** methods and properties. Note that it is up to the programmer to implement the actual code when conforming to a protocol.

* **DataSources**: You implemented the data source protocol to populate your table view with your note's data, which was stored in realm's local storage.

* **Extensions**: Extensions add new functionality to an existing class, structure, enumeration, or protocol type. In the notes app, we added our delegates as extensions. Note that you could add delegate support directly to the class; however, it looks a bit nicer as an extension. 

###Data Persistence

* **Persistence**: Persistence is the ability to save data - it allow us to close an app and reopen it without losing anything.

* **Realm**: You used Realm to implement a data persistence layer. We learned about Realm entities, setting up our own `Note` model, and how to read and write this information.

###Keyboard Handling & Constraints

* **Text Input**: Adding text input functionality to the app and customising the experience.

* **Keyboard Notifications**: Having the keyboard inform our code when the keyboard was going to appear and when it was going to disappear.

* **Constraints**: Using constraints to ensure the toolbar was constrained to the bottom space of our app.

* **Animating Constraints**: Using keyboard notifications, along with constraints, to ensure the toolbar stays above the keyboard.

###Note Lifecycle

* **Create Note**: You created new notes by passing an empty Note object through to the New Note Controller to be populated by the user.

* **Display/Modify**: You learned how to display the note and how to allow the user to modify existing note information.

* **Remove Notes**: You implemented a mechanism to remove notes through the trash can and using table view behavior (swipe to delete).

###Search

* **Search Bar**: A search bar was added to enable filtering of note data.

* **State Machine**: A state machie was implemented to give us a `.DefaultMode` and `.SearchNode` so the NotesViewController behaviour could be modified based on state.

###Thoughts

A lot of ground was touched upon here and these concepts are a great step towards building a bigger app.

If you take a popular app like "Yelp", you have a table view, a search bar and a list of results. 
The cell view may have an image and a few more fields; however, you could easily add an image and more information to your cell.

You used local storage of user data; however, imagine you were using cloud storage populated with restaurant data...

Notes may be a simple app; however, it's only a few iterations away from something much more.
