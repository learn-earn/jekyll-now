---
published: false
---
## How to architect my iOS project

In the early stage of my iOS App development career, I was following Paul Hegarty from Stanford University(Thanks Paul, well done!); and reading [Apple Developer Documentation](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/DevelopiOSAppsSwift/). 

I still remember that we were all talking about MVC (Model-View-Controller). It is a design pattern that helps us to separate UI elements(View) from Business logic (Model), handle users interactions, app lifecycles from the Controller, and using KVO, delegate pattern to notify view the data changes etc. Segues, view navigations using storyboard etc. All the good stuff.

However, after a several years of experiences with MVC, especially with Apple's ViewController. I feel like it's difficult to write unit tests, and often I am fighting against Apple's framework to create my tests. My ViewController became enormously huge. It owns many other responsiblities which it should have owned. For instance, networking, navigation, database, etc....

What's happening? Why do this problem occur?

I took a step back to read about MVC and many other popular architecture design patterns among iOS developers, then I realied that those design patterns (MVC/MVVM/MVP) are UI related architecture design patterns. They are good at solving UI related problems.

Then how do we acheive a better architecture for our apps? I spent another few years looking for solutions from online blogs, video tutorials and tech conferences. It got me to rethink about SOLID principle and modular designs. 
