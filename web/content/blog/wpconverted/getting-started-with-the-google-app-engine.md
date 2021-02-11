---
title: 'Getting started with the Google App Engine.'
date: Sun, 18 May 2008 11:30:43 +0000
draft: false
tags: ['code', 'maps', 'technology']
---

The tutorial goes over some basic concepts surrounding Google's App Engine Framework, demonstrates using the Google App Engine to store data, and using Django templates to create a GeoRss feed that is consumed by Google maps. **Setup your environment** I chose eclipse as my ide. The nice thing about eclipse is if you add the lib directories of whatever you are using (including the Google App Engine) you will get some intellisense. Download the necessary components. [Google app engine](https://www.google.com/accounts/ServiceLogin?service=ah&continue=http://appengine.google.com/_ah/login%3Fcontinue%3Dhttp://appengine.google.com/&ltmpl=ae&sig=2441550b0617bb4eb9e7f8c3eb9e63b1) [Eclipse](http://www.eclipse.org/downloads/) [Installing Pydev](http://www.fabioz.com/pydev/manual_101_install.html) The documentation helped, but the link was bad. I used http://pydev.sf.net/updates/. **The guts of the python file**```

import cgi
import os
import wsgiref.handlers

from google.appengine.api import users
from google.appengine.ext import webapp
from google.appengine.ext import db
from google.appengine.ext.webapp import template

\_DEBUG=True

class Business(db.Model):
    ......

def main():
    application = webapp.WSGIApplication(\[
            ('/',MainPage),
            ('/createbusiness.do',BusinessSignup),
            ('/georssfeed.xml',GeoRssFeed)
            \],debug=\_DEBUG)
    wsgiref.handlers.CGIHandler().run(application)

if \_\_name\_\_ == "\_\_main\_\_":
    main()


```The main method is where we map our urls to the classes we have defined within the python file. Each class that handles requests should have a get or a post method. When a get or a post occurs it will be routed automagically to the appropriate method. **Creating the table**```
 
class Business(db.Model):
    name = db.StringProperty()
    description = db.StringProperty(multiline=True)
    url = db.URLProperty()
    location = db.StringProperty()
    latitude = db.StringProperty()
    longitude = db.StringProperty()
    address = db.StringProperty()
    created = db.DateTimeProperty(auto\_now\_add=True)

```Description can contain line breaks so we specify multiline=True Created is of type DateTime and has the property auto\_now\_add set to true created is set to the current time the first time the model instance is stored in the datastore, unless the property has already been assigned a value. There is also an auto\_now property that can be used to set the current time each time the record is created or updated. Useful for modified dates. **Handling the request** In one of googles examples(Task List) they used a base class for the request. Here is my modified version.```
class BaseRequestHandler(webapp.RequestHandler):
    """Supplies a common template generation function"""
    def generate(self,template\_name,template\_values={}):
        values = {
                  'request': self.request,
                  'debug': self.request.get('deb'),
                  'application\_name': 'Local Business Directory'
                  }
        
        values.update(template\_values)
        directory = os.path.dirname(\_\_file\_\_)
        path = os.path.join(directory,os.path.join('templates',template\_name))
        self.response.out.write(template.render(path,values,debug=\_DEBUG))

```This does a few nice things.```
        values = {
                  'request': self.request,
                  'debug': self.request.get('deb'),
                  'application\_name': 'Local Business Directory'
                  }
        values.update(template\_values)

```This sets up an array of base values that will be passed into the template. In other methods that use base request, we will add other objects to this array. So our html templates can process data. The last line values.update is where the two arrays gets merged.```
        path = os.path.join(directory,os.path.join('templates',template\_name))

```In this application I created a templates folder to separate the html from the code. This line just adds the template\_name to the /templates path.```
    self.response.out.write(template.render(path,values,debug=\_DEBUG))

```And finally Write the request out. Using the BaseRequestHandler```
class MainPage(BaseRequestHandler):
    def get(self):
        
        #Get all of the businesses
        businesses = Business.all().order('-created')
        
        self.generate('index.html', {
                                     'businesses': businesses
                                     })   

```Here is a simple example of querying all of the businesses ordered by created date. We then call the generate method on the BaseRequestHandler, passing in our additional objects, along with the template name. **Using Templates** The Google App Engine uses the Django templating engine. W00t The for loop```
{% for athlete in athlete\_list %}
    *   {{ athlete.name }}
{% endfor %}

```The if statement(there are several varieties)```
{% if athlete\_list %}
    Number of athletes: {{ athlete\_list|length }}
{% else %}
    No athletes.
{% endif %}

{% ifequal user.id comment.user\_id %}
    ...
{% endifequal %}

```In the spirit of python, there are a lot of functions that Django gives you. Examples: timesince: Formats a date as the time since that date (e.g., “4 days, 6 hours”). phone2numeric: Converts a phone number (possibly containing letters) to its numerical equivalent. For example, '800-COLLECT' will be converted to '800-2655328'. [More Information on Django templates](http://www.djangoproject.com/documentation/templates/) In order to display a list of businesses I am just using a simple for loop and creating a row each time.```


	
		{% for business in businesses %}
		
		<td class="main" 
		  

{{ business.name }}

		{% endfor %}
	

		

			

Business Name

			

Address(Address, City, State)

			

Description

			

Url

			

Location

			

Latitude

			

Longitude

		

		

		
		

			{{ business.address }}
		

		

			{{ business.description }}
		

		

			{{ business.url }}
		

		

			{{ business.location }}
		

		

			{{ business.latitude }}
		

		

			{{ business.longitude }}
		

		
		


```**Entering Data** Two pieces of code were necessary for this Plumbing in the python file```
class BusinessSignup(webapp.RequestHandler):    
    def post(self):
        business = Business()
        
        business.name = self.request.get("txtBusinessName")
        business.address = self.request.get('txtAddress')
        business.description = self.request.get('txtDescription')
        business.url = self.request.get('txtUrl')
        business.location = self.request.get('txtLocation')
        business.latitude = self.request.get('txtLatitude')
        business.longitude = self.request.get('txtLongitude')
        
        business.put()
        self.redirect('/')

```This just grabs from the data from the request and sets each property on our business object. Then calls put. put is an instance method that saves the data to the database. delete, to\_xml, is\_saved, are a couple of other useful instance methods.

Tells the form to post to the specified address. **Bringing it all together**```
application: yourapplication
version: 1
runtime: python
api\_version: 1

handlers:
- url: /static
  static\_dir: static

- url: /.\*
  script: localbusinesslocator.py

```The app.yaml is where your external url mapping occurs. If you wanted to use several python files, this is where that would happen. More Info can be found [here](http://code.google.com/appengine/docs/configuringanapp.html) Testing the application usr/local/google\_appengine/dev\_appserver.py /sourcedirectory/ Hopefully this fills in some gaps left by [Googles tutorial.](http://code.google.com/appengine/docs/gettingstarted/) The next installment of the series will go over displaying the data in the GeoRss format and displaying it on google maps.