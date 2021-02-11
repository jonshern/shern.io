---
title: 'Data Analysis with Python - The Tools and the Data'
date: Wed, 09 Sep 2015 23:16:46 +0000
draft: false
tags: ['CodeProject', 'machine learning', 'python']
---

**Welcome to the Inaugural Post for my Learning Python Series** The Post is the start of a series of walk throughs from Start to Finish of my journey into Data Analysis and Data Science with Python   **The Tools and Loading the Data** What are the tools to download in order to get started building in Python How do I load the data and construct the domain How do I do some basic analysis on the data to get a feel for the relationships. **The Next Part** The next post will utilize Panda to perform quicker more structure data analysis.     **The Tools** I downloaded and installed the [Anaconda](https://store.continuum.io/cshop/anaconda/) distribution along with the [Visual Studio Python Tools](https://www.visualstudio.com/en-us/features/python-vs.aspx) The Anaconda distribution is a python distribution that contains many many scientific libraries. **The Data** Fortunately there is a massive amount of data that you can have fun and experiment with. The [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/index.html) has a huge amount of data. Examples

*   [Mice Protein Expression](http://archive.ics.uci.edu/ml/datasets/Mice+Protein+Expression)
    *   Expression levels of 77 proteins measured in the cerebral cortex of 8 classes of control and Down syndrome mice exposed to context fear conditioning, a task used to assess associative learning.
    *   \[caption id="" align="alignright" width="166"\]![](https://jonsherndotcom.files.wordpress.com/2015/09/390b2-micegfp.jpg) Mice Protein\[/caption\]
*   [Car Evaluation Data Set](http://archive.ics.uci.edu/ml/datasets/Car+Evaluation)
    *   Derived from simple hierarchical decision model, this database may be useful for testing constructive induction and structure discovery methods.
*   [Adult Data Set](http://archive.ics.uci.edu/ml/datasets/Adult)
    *   Predict whether income exceeds $50K/yr based on census data. Also known as "Census Income" dataset.

I am going to use the Adult Data Set in my Examples.  

**Loading the Data and Graphing the Data**
------------------------------------------

Grab the Data from [http://archive.ics.uci.edu/ml/datasets/Adult](http://archive.ics.uci.edu/ml/datasets/Adult)```
import csv

with open('C:\\adult.test','r') as f:
    for line in f: 
        reader = csv.reader(f)
        for row in reader: 
            age = row\[0\]
            workclass = row\[1\]
            fnlweight = row\[2\]
            education = row\[3\]
            educationnum = row\[4\]
            maritalstatus = row\[5\]
            occupation = row\[6\]

            print workclass

```This will print out the workclass column in the data. Resulting in an output like```
 State-gov
 Federal-gov
 Private
 Private
 Private
 Local-gov
 Private
 Local-gov
```In order to start to looking into the data we can use someone of Python's built in magic to bucket the data and create some histograms. Histograms will tell us how the data is distributed and start to give us clues about the shape of the data. In order to create a histogram we will use the [collections](https://docs.python.org/3/library/collections.html) library to count the data  ```
def create\_histogram(labels, values, bucket\_size, title):
    plt.bar(labels, values)
    plt.title(title)
    plt.show()
    

agelist = list()

with open ('C:\\Users\\Jon\\Documents\\adult.test','r') as f:
	for line in f:
		reader = csv.reader(f)
		for row in reader:
			try:
				age = row\[0\]
				agelist.append(age)

			except IndexError:
				print("something")

agelistfloat = \[float(x) for x in agelist\]
agedist = Counter(agelist)

labels, values = zip(\*agedist.items())

valueslistfloat = \[float(x) for x in values\]
labelsliststring = \[float(x) for x in labels\]

create\_histogram(labelsliststring,valueslistfloat,5,"Age Distribution Simple")
```Once I get the file I take my agelist and run it through Counter. Counter allows for rapid tallying of data.  It returns a defaultdict object that list each age and how many occurences there were. We then unzip the list to labels and values.  \* reverses the zip operation. We then call the pyplot.bar(labels, values) to show the graph. ![Simple Histogram](https://jonsherndotcom.files.wordpress.com/2015/09/simpleagedistribution.png?w=660) We can see that our data is a [right-skewed distribution](http://asq.org/learn-about-quality/data-collection-analysis-tools/overview/histogram2.html).  When you look at the data you can see it is evenly distributed over the income generating population.  At around 18 it starts and starts to gradually tail off at the peak of around 40.  

**First Refactoring - Making the code a bit more compact and readable**
-----------------------------------------------------------------------

I mapped each row to a named tuple in order to iterate through the data a bit more intuitively First I created a dictionary of the columns in the data.```
    economic\_columns = \['age', 'workclass', 'fnlwght', 'education', 'educationNum', 'maritalStatus', 'occupation', 'relationship', 'race', 'sex', 'capitalGain', 'capitalLoss', 'hoursPerWeek','nativeCountry', 'income'\]

    EconRecord = collections.namedtuple('econ',economic\_columns)
```  I then created a object that would represent the named tuple.```
    rowlist = list()

    for econ in map(EconRecord.\_make, csv.reader(open('C:\\Users\\Jon\\Documents\\adult.txt', "r"))):
        rowlist.append(econ)
```I used the EconRecord and the map function to apply EconRecord.\_make to every record in the collection.  Creating a new econ record for each row in the file. The result is being able is being to aggregate the items in a bit more cleanly with more concise readable code.```
    agelistint = \[int(x.age) for x in rowlist\]
    agedist = Counter(agelistint)
    labels, values = zip(\*agedist.items())
```The Next Series we will start to scatter plot and look for relationships in the data using Panda.   **References ** [Python Lists](http://effbot.org/zone/python-list.htm) The Book - [Data Science from Scratch](http://www.amazon.com/Data-Science-Scratch-Principles-Python/dp/149190142X) [Beautiful Plots With Pandas and Matplotlib](https://datasciencelab.wordpress.com/2013/12/21/beautiful-plots-with-pandas-and-matplotlib/) [Collections ](https://docs.python.org/3/library/collections.html) [CSV ](https://docs.python.org/3/library/csv.html) - Python [Python Structure ](http://docs.python-guide.org/en/latest/writing/structure/)