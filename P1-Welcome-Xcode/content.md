---
title: "Welcome to Xcode"
slug: welcome-xcode
---     

Xcode is Apple’s integrated development environment (IDE). Xcode includes a source editor, a graphical user interface editor, and many other features. 
It's a fantastic development tool for iOS development and one that you will be making frequent use of, you've only just scratched the surface while 
experimenting in the Swift Playground.

If you would like to learn more about Xcode in general, then Apple has you covered. [developer.apple.com/xcode](https://developer.apple.com/xcode/)

##Let's get started

Open up Xcode and you will be presented with the following options:

![image](xcode_started_1.png)

Notice the *Version* 6.3.1, Swift is a new language and is still maturing with every release so you will require at least 6.3.X.
The easiest way is to install Xcode via the Mac App Store which will always keep you up to date with the latest release.

##Your first project

Select *Create a new Xcode project*

You will then be presented with the following project options:

![image](xcode_started_2.png)

As you can see there are a lot of options hoewver let's take it easy and start off by selecting `Single View Application` and select *Next*.

##Project Options

You will now be presented with the new project options:

![image](xcode_project.png)

Feel free to give it whatever ProductName or Orginization Name you like, it's your project after all. 
Please ensure you selected *Swift* as the choice for *Language*, then select *Next*.

##Source Control

Time to save your project to disk:

![image](xcode_project_2.png)

I would recommend enabling the *Create Git repository* option at the bottom of the dialog.  For a project this size it may not be strictly necessary however it's a great habit to get into
and source control has saved many a developers life.  We will explore Source Control with Git a bit later on in this tutorial.

If you would like some further reading on the joys of Git, you should check out the [Wikipedia Git](http://en.wikipedia.org/wiki/Git_%28software%29)

##Run, Run, Run

It's always good to check that a project works before starting any new work, simply hit the *Play* icon to *Build* and *Run* the application.

![image](xcode_run.png)

That was quick...

![image](simulator_1.png) ![image](simulator_2.png)

The first image shows the loading *View* and then loads in our default *ViewController*, as expected it's blank canvas crying out to be worked on.

Let's move on and take a look at the *Interface Builder*.