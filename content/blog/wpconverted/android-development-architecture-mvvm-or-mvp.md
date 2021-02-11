---
title: 'Android Development - Architecture (MVVM or MVP)'
date: Sat, 21 Jan 2012 23:10:11 +0000
draft: false
tags: ['android', 'code', 'mvvm']
---

Normal 0 false false false EN-US X-NONE X-NONE /\* Style Definitions \*/ table.MsoNormalTable {mso-style-name:"Table Normal"; mso-tstyle-rowband-size:0; mso-tstyle-colband-size:0; mso-style-noshow:yes; mso-style-priority:99; mso-style-parent:""; mso-padding-alt:0in 5.4pt 0in 5.4pt; mso-para-margin-top:0in; mso-para-margin-right:0in; mso-para-margin-bottom:10.0pt; mso-para-margin-left:0in; line-height:115%; mso-pagination:widow-orphan; font-size:11.0pt; font-family:"Calibri","sans-serif"; mso-ascii-font-family:Calibri; mso-ascii-theme-font:minor-latin; mso-hansi-font-family:Calibri; mso-hansi-theme-font:minor-latin; mso-bidi-font-family:"Times New Roman"; mso-bidi-theme-font:minor-bidi;}

**Some Background**

Most of my experience has been doing .net development. For the last few years I have done client development: WPF, Silverlight, and Windows Forms. Prior to that I was doing mostly asp.net development.

I recently started getting my feet wet in Android development and one of the first steps in any platform is to get comfortable with the parts of the applications and how they talk to each other.

Android is new to me so I will define some terms to start off with.

**Activities**: application component that provides a screen with which users can interact in order to do something.

**Layout**: A layout is the architecture for the User Interface. Either defined by XML or in Code.

**Intents**: Messaging object that can be used inside of an application or from application to application.

The Xml layouts allows for methods to be implicitly wired to methods in the Activity that loaded the resource. Similar to Commanding in XAML.

How do you classify an Activity in terms of UI patterns?  
Is it a Controller, View, Model, ViewModel, Presenter, does it fall outside of a UI Pattern, or is it a combination of many?

**Is it a Controller?**  
In an MVC application you have a request come in and the controller handles the routing. It is pretty clear example of a controller. It is entirely possible that you could write the app in a way that when an Activity is called it determines the view, but this seems like a stretch. I would see the relationship between an Activity and a View to be One-to-One.

In this case I would see the OS being the [Controller](http://msdn.microsoft.com/en-us/magazine/hh580734.aspx).

So in my opinion it is not a Controller.

**Is it a View?**

Since you can create UI’s in an Activity using code, you could have a situation where it acts like a view, but you would lose the separation of concerns gained by using XML.

![BasicAndroidArchitecture](http://jonshern.com/wp-content/uploads/2012/01/BasicAndroidArchitecture.png "BasicAndroidArchitecture")

**Is it a Presenter or a ViewModel?**

If you consider the notification and management of the listadapter as Binding then it would be a ViewModel

ArrayAdapter<Customer> adapter = **new** ArrayAdapter<Customer>(**this**,android.R.layout._simple\_list\_item\_1_,customers);

setListAdapter(adapter);

adapter.notifyDataSetChanged();

If you don’t consider this a form of binding then it is probably more of a presenter.

In my opinion it feels more like MVVM but then my background in WPF could be skewing my opinion.

**References**

[MVVM vs. MVP vs. MVC  
](http://joel.inpointform.net/software-development/mvvm-vs-mvp-vs-mvc-the-differences-explained/)