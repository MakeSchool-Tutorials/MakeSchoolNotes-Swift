---
title: "Connecting Objects"
slug: connecting-objects
---     

The time has finally come to start connecting the dots and connect objects from the Interface Builder to our own code. 
Your code will connect with interface objects through IBAction and IBOutlet connections.

##IBAction

You will create an `IBAction` when you need to send a message from an object to your code, a good example is when a user clicks a button.  This action by the user
will send a message to execute the function that you connected to. 

Open up `ViewController.swift` and add the following method inside your class.

```
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    // Let's handle the button
    @IBAction func buttonTapped(sender: AnyObject) {
        println("Ouch")
    }
   
}
```

Great you just created a function that simply prints out a line of text however by adding the *@IBAction* attribute to the function defintion it will now be accessible to the Interface Builder.
Notice the little empty circle in the code editor to the left of this function defintion...

If you select the `Assitant Editor` view you want to see the following:

![image](ibaction_connection_1.png)

If you don't, most likely you will want to change the right panel back to `Automatic`, you previously changed it to enable the Interface Builder preview mode.

![image](automatic_view.png)

You now want to *Connect* your *IBAction* to the *Button*, this is straightfoward:
 
`Control-Click` onto the circle beside the *@IBAction* and drag this across to your `Button` you will see a light blue line as you 
drag across.

![image](ibaction_connection_2.png)

That was pretty easy...
Notice the previously empty circle is now filled in, move your cursor over this circle and it will highlight the `Button` showing the connection.
You can also check out any Connections via the `Connection Inspector`

![image](connection_inspector_1.png)

Great, let's see this in action.  Time to run the app...

Click the Button and booom, the *Debug* area will now appear with a few `Ouch` messages for good measure.

![image](debug_1.png)

Excellent young padawan, let's see how you handle yourself and move onto outlets.

##IBOutlet

You will want to use an *IBOutlet* to enable your code to send a message to your user interface object, for example changing the string of an interface label from your own code.

Time to try this out for yourself, see if you can:

1. Add a label to your View 
2. Create a variable with attribute *IBOutlet* in your `ViewController` class that you will connect to your *UILabel*
3. Connect your View's Label to your code's label variable.

**Bonus Points**
1. Use Auto Layout
2. Make your Label change text when the `buttonPressed` function is called.

<div class="solution" markdown="1" title="Adding an IBOutlet">
Your view *ViewController* should look something like this:
```
class ViewController: UIViewController {
    
    @IBOutlet var label: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    // Let's handle the button
    @IBAction func buttonTapped(sender: AnyObject) {
        println("Ouch")
    }
}
```
</div>



