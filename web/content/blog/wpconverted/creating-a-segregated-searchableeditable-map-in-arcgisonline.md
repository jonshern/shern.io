---
title: 'Creating a Segregated Searchable/Editable Map in ArcGISOnline'
date: Sun, 22 Nov 2015 14:00:02 +0000
draft: false
tags: ['CodeProject', 'gis', 'maps', 'technology']
---

I would like to create a map that stores core company location data, that anyone in the company can search and a select few can add data points [ArcGISOnline](http://www.arcgis.com/features/) has many features to jump start your project.

The Architecture/Plan
---------------------

![SolutionArchitecture](https://jonsherndotcom.files.wordpress.com/2015/11/solutionarchitecture.png?w=660)

### **Basic Parts**

These are my interpretations of the pieces\*

*   Feature Layer
    *   A Feature Layer is the hosted data.  It is where you would upload your shapefile/geodatabase.
    *   It is the data store.  A Web Map is made up of one to many Feature Layers.
*   Web Map
    *   It the most basic viewer of the Feature Data.  It is where you add your features and define how your points show up.
*   [Web Mapping Application](http://doc.arcgis.com/en/web-appbuilder/)
    *   There are two ways to create
    *   Web App Builder for ArcGIS
        *   Allows you add various widges
    *    [Templates](http://www.arcgis.com/home/gallery.html#c=esri&t=apps&o=modified&f=configurable)
        *   A template is a pre-built website that has a purpose built function.  ESRI has put all of their [templates on GitHub](https://github.com/Esri) so you can download and customize them in order to meet your mapping needs.
        *   Examples
            *   Basic Viewer - Presents a map in a general purpose app with a collection of essential tools including edit and print.  [Download](https://github.com/Esri/Viewer)
            *   Compare Analysis - Compares geographic phenomena at one or more locations with multiple side-by-side maps.  [Download](https://github.com/Esri/Compare-Maps-Template)
            *   Crowdsource Manager - Provides the ability to review crowd sourced information and update attributes such as status, assignment, etc. [Download](https://github.com/Esri/crowdsource-manager)

![Domain](https://jonsherndotcom.files.wordpress.com/2015/11/domain.png)

Lets Get Started
----------------

* * *

 

### **Step One**

Create a Layer that will store the data. Since I live in Minnesota I created a Shapefile with the Coordinate System

Projected Coordinate System: Name: NAD\_1983\_UTM\_Zone\_15N

Geographic Coordinate System: Name: GCS\_North\_American\_1983

Here is the Data I am working with

![ShapefileData](https://jonsherndotcom.files.wordpress.com/2015/11/shapefiledata.png?w=660) I created Attributes for ProjName, CreateDate, Company, ProjType So we have some good things to search for once the data is uploaded.   The First Step is to zip up my shape file data. It is all of the data that begins with your shape file name. ![UploadShapefile](https://jonsherndotcom.files.wordpress.com/2015/11/uploadshapefile.png?w=660) In My Content go to Add Item / Select My Computer and choose the zip file you just created. Now we have a Feature Layer that we can use as a foundation for our web maps and templates. You can go into the Feature Layer click on Edit and Check the box for "Enable editing and allow editors to: Add, update, and delete features" Note: You can re-upload an updated shape file by clicking on overwrite and picking a file.  This is also a nice way to manage updates if you don't need a way for novices to edit the data.

* * *

 

### **Step Two**

In the Content Area go to Create Now create a new Map.  -- This will create a new Web Map This will bring into Edit Mode.  Go to the top of the now editable web map and add the layer we just created in the previous step. ![MonumentWebMap](https://jonsherndotcom.files.wordpress.com/2015/11/monumentwebmap.png?w=660) Save it

* * *

 

### **Creating the Editor**

I initially tried the template Basic Editor.  I found this template to be pretty useful in terms of editor. The nice part is you can add points and edit the data. The Search is not very good, when you enable it.  If you search for a feature like Company Name, it will go to the first point it finds, and you cannot go to the next result. In contrast the Find/Edit/Filter Template works great for filtering, but does not allow you to add points. Trade-offs:( Due to the trade-offs, and some of the weird complexity I decided to use the Web App Builder for ArcGIS.  This allowed me to customize the filters and Edit Widget. I am sure the downside to the Web App Builder is the code is not available so I can't create a brand new variant. I have no desire to do that, so this should be good. The Filter Setup is a breeze, and once it is done it looks great.  You can even customize how the results look.  A drawback on some of the templates.   ![Filter Setup](https://jonsherndotcom.files.wordpress.com/2015/11/filter-setup.png?w=660) ![ResultsFilter](https://jonsherndotcom.files.wordpress.com/2015/11/resultsfilter.png?w=660) Once we are finished you have a nice filter interface with a marginally slick editor interface(who are we kidding, editing a map nicely in an web app is a pretty hard task) ![WebAppBuilder_EditFilter](https://jonsherndotcom.files.wordpress.com/2015/11/webappbuilder_editfilter.png?w=660)

* * *

  **Creating the Searchable Viewer** I had tried using some of the Filter Templates At the end of the day I found the Web App Builder worked great. I just created a Web App Builder and did not include the edit widget. In this case I could just use sharing to manage who had permissions to what functions.   At this point I am done. I created the feature layer, Web Map, and two web apps that should expose segrated edit/filter capabilities to your different user types.