---
title: 'Adding properties to the App Class'
date: Fri, 12 Feb 2010 05:56:20 +0000
draft: false
tags: ['code', 'silverlight', 'wpf']
---

If you want to add properties to your App class (App : Application) and actually access them in your application.

Make sure to override Current to return an App instead of an Application.

       
        public static new App Current  
        {  
            get  
            {  
                return Application.Current as App;  
            }  
        }