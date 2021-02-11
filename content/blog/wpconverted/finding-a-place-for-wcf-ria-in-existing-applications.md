---
title: 'Finding a place for WCF RIA in existing applications'
date: Thu, 17 Dec 2009 23:43:56 +0000
draft: false
tags: ['code', 'entity framework', 'poco', 'ria', 'silverlight', 'silvlerlight']
---

Works with Entity Framework 4.0 CTP 2

My application so far ...

The premise of this application is to create a way for one or more people to create a common practice routine, set goals, and compare their progress.

I started this application with the intention of using Entity Framework 4 and RIA.

I started having problem with inserting from the server side, so I decided to start with a more basic model, and work my way into RIA and EF.

My data model looks like this

[![clip_image002](http://jonshern.com/wp-content/uploads/2009/12/clip_image002_thumb.jpg "clip_image002")](http://jonshern.com/wp-content/uploads/2009/12/clip_image002.jpg)

I started with the problem of how do I save disconnected entities across a service boundary.

I found that converting to Self Tracking Entities solved this problem very elegantly.

###### Problem Number 2

###### Option 1(The hard way)

I want the entire object graph on the client side, and I would like to be able dynamically generate a graph using a collection of Task Items.

I could do this by creating a silvlerlight class library sharing the objects(once they are converted to POCO’s) and using linking to share the objects between layers.

I would then have to build up the structure on the client side, using a series of async loads.

This is a lot of work. I have done it before, and every time I do it, I feel like I just wasted time I will never get back.

###### Option 2 - Use RIA and POCO.

I know that RIA sits in between the Server and the Client

[![clip_image004](http://jonshern.com/wp-content/uploads/2009/12/clip_image004_thumb.jpg "clip_image004")](http://jonshern.com/wp-content/uploads/2009/12/clip_image004.jpg)

(Shamelessly stolen from Brad Abrahams excellent RIA Services video)

**So how do I add RIA to this project?**

**Setting up your POCO entities**

I have an entity class for Tasks

```
namespace PracticeManager.Common.Entities
{
    public class Task {
        \[Key\]
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime CreatedTime { get; set; }
        public virtual DateTime ModifiedTime { get; set; }
        public virtual string Status { get; set; }
        public virtual string MeasurementUnit { get; set; }
        public virtual string UserName { get; set; }
        public virtual int? CommentId { get; set; }

        public ICollection<TaskItem\> TaskItems { get; set; }
        public ICollection<Goal\> Goals { get; set; }
        public Comment Comment { get; set; }
        public ICollection<Taunt\> Taunts { get; set; }
        public ICollection<Challenge\> Challenges { get; set; }
        public ICollection<Challenge\> Challenges\_1 { get; set; }

    }
}
```[](http://11011.net/software/vspaste)

I have my context class.

```
namespace PracticeManager.Server.Business.DataContext
{
    public class TaskDataContext : ObjectContext {
        private ObjectSet<Task\> \_tasks;

        public TaskDataContext(string connectionString)
            : base(connectionString, "PracticeModelContainer")
        {
          
        }


        public ObjectSet<Task\> Tasks
        {
            get {
                return \_tasks ?? (\_tasks = base.CreateObjectSet<Task\>());
            }

        }

    }
}
```[](http://11011.net/software/vspaste)

This covers the data loading portion.

**Hooking up RIA Services**

In your Web Project add a Domain Service class.

A wizard will appear, I avoided the wizard. I want to do this programmatically.

My domain class is pretty simple, for now I just want a list of tasks.

```
\[EnableClientAccess()\]
public class PracticeManagerDomainService : DomainService {
    private TaskDataContext taskContext;

    public IEnumerable<Task\> GetTasks()
    {
        var cnxString = ConfigurationManager.ConnectionStrings\["PracticeModelContainer"\].ConnectionString;
        taskContext = new TaskDataContext(cnxString);
        var tasks = taskContext.Tasks;

        return tasks;
    }
}
```[](http://11011.net/software/vspaste)

This is far from pretty, I need to move my connection string out of the method, but this will work.

**On the client side**

In my code behind I have two methods.

```
public void LoadTasks()
{
    EntityQuery<Task\> taskQuery = \_practiceManagerDomainContext.GetTasksQuery();
    \_practiceManagerDomainContext.Load(taskQuery, this.OnTasksLoaded, null);
}

private void OnTasksLoaded(LoadOperation<Task\> loadOperation)
{
    nameComboBox.ItemsSource = \_practiceManagerDomainContext.Tasks;
}
```

This takes care of the async load and connecting to the service.

The Task object comes across because of its exposure in the domain service class.

The namespace is even the same as the server side, which indicates it is the same object. Not just a service proxy.

Once I call the LoadTasks method, I will get a dropdown populated with tasks.

That is all I need to do.