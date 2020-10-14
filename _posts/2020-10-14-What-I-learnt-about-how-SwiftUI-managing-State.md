---
published: true
layout: post
title: What I learnt about how SwiftUI managing State 
excerpt: >-
  What do we do if we need to change the property values in view using SwiftUI? 
---

In SwiftUI, views are struct, and struct is immutable. So we cannot change the value of its properties.

### What do we do if we need to change the property values in view using SwiftUI? 

SwiftUI allows us using `@State` property wrapper to turn the immutable property to mutable. Recommendation from Apple,
properties with `@State` property wrapper are used for simple values such as String, int, boolean. And we should only allow 
the view to access these properties. So we usually mark these properties `private`.

Once the value of these properties with `@State` changes, SwiftUI will notify the view to reload the entire screen.

### How do we update those properties values changes when the values are changed by the UI?

In SwiftUI, we prepend `$` to the property, to achieve this. The property value changes will notify the UI to update, and 
when UI is changing, it also updates the property, this is called 2 way binding.

### Another property wrapper called `@EnvironmentObject`, when do we use it?

`@State` is used for the view which owns the property. If we want to pass the property value on to another view, then we will use
the `@EnvironmentObject`. 

SwiftUI creates a key-value store in the global environment, before we can access it from another view, we will need to create it and add to the global environment.
Not all the views can access the value of `@EnvironmentObject`, only its children views can access it.

### The third property wrapper we uses in SwiftUI is `@ObjectBinding`. 

We use this to share complex properties like custom types, usually reference type in many views.

For example, in view A, we need to change the `name` property of custom type `Student`, since `Student` is a class, so SwiftUI cannot notify another View, which reference to the same model to get the changes. Then view A and view B which reference to the same model will be out of sync.
In this scenario, we can use [Apple's Combine framework](https://developer.apple.com/documentation/combine) to notify view about the model changes.

* First step, `Student` will need to conform to `BindableObject` and we add the `@ObjectBinding` property wrapper to the property referencing to custom type.
* Second step, we need to initialise an object from the custom type.
* Lastly, we can pass it to any view using `.environmentObject(instanceOfTheCustomType)`

The value changes of the `instanceOfTheCustomType` will be automatically passed on to any view which references to it.

## Reference
* [Swift tutorial: What's the difference between @State, @ObjectBinding and @EnvironmentObject? - by Paul Hudson](https://www.youtube.com/watch?v=stSB04C4iS4&ab_channel=PaulHudson)
* [Apple's combine framework](https://developer.apple.com/documentation/combine)
* [Swift Property Wrapper](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)
* [SwfitUI Property Wrapper](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)
