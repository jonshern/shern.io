---
title: 'Using Self Tracking Entities in Entity Framework 4 to fix the error - The object could not be added or attached ...'
date: Wed, 09 Dec 2009 22:37:23 +0000
draft: false
tags: ['code', 'entity framework']
---

Works with Entity Framework 4 CTP 2 and Visual Studio 2010 Beta 2

{"The object could not be added or attached because its EntityReference has an EntityKey property value that does not match the EntityKey for this object."}

I am able to to insert the first record.

But subsequent records give the above error.

The structure is: Tasks have Task Items. I am trying to save task items.

 In this example my silverlight application is initiating this service call.  
The task object is set by a dropdown on the client  side.

```
        

        public void InsertTaskItem(Task task, string value, DateTime dateTime)
        {
            TaskItem taskItem = new TaskItem();

            taskItem.Task = task;
            taskItem.TaskId = task.Id;
            taskItem.Value = value;
            taskItem.OccurrenceTime = dateTime;

            taskItem.CreatedTime = DateTime.Now;
            taskItem.ModifiedTime = DateTime.Now;

            this.objectContext.AddToTaskItems(taskItem);
            this.objectContext.SaveChanges();
        }



```

First Attempt Lets just atttach the object, it should be that easy right?

```
        
        public void InsertTaskItem(Task task, string value, DateTime dateTime)
        {
            TaskItem taskItem = new TaskItem();

            taskItem.Task = task;
            taskItem.TaskId = task.Id;
            taskItem.Value = value;
            taskItem.OccurrenceTime = dateTime;

            taskItem.CreatedTime = DateTime.Now;
            taskItem.ModifiedTime = DateTime.Now;

            objectContext.AttachTo("PracticeManager.Server.Business.Data.PracticeModelContainer.TaskItem", taskItem);
            this.objectContext.SaveChanges();

        }
      

```

  
I received the error.

```
The provided EntitySet name must be qualified by the EntityContainer name, such as 'EntityContainerName.EntitySetName', or the DefaultContainerName property must be set for the ObjectContext.
Parameter name: entitySetName

```

I tried both the fully qualified type name, and the entity set name.

  
Since I am using .Net 4.0 and the Entity Framework,  I realized I can use some of the POCO functionality they added to facilitate this specific scenario.

I can refactor and use Self Tracking Entities to accomplish my goal the right way.

The first step is download the [Entity Framework 4 CTP 2](http://www.microsoft.com/downloads/details.aspx?familyid=13FDFCE4-7F92-438F-8058-B5B4041D0F01&displaylang=en)  - It did not make it into VS 2010 Beta 2.

Navigate to your model, and Add New Code Generation Item.

Select ADO.NET Self Tracking Entity Generator.

```
        public void InsertTaskItem(Task task, string value, DateTime dateTime)
        {
            TaskItem taskItem = new TaskItem();

            taskItem.Task = task;
            taskItem.TaskId = task.Id;
            taskItem.Value = value;
            taskItem.OccurrenceTime = dateTime;

            taskItem.CreatedTime = DateTime.Now;
            taskItem.ModifiedTime = DateTime.Now;


            using (var context = new PracticeModelContainer())
            {
                context.TaskItems.ApplyChanges(taskItem);
                context.SaveChanges();
            }
        }

```

Once I added the context save changes, the task item now save.

One problem is when setting the task property it is triggering the creation of a new entity, and then overwriting the foreign key with the key of the newly created task.

Once the line

```
 
taskItem.Task = task;

```

is commented out the save works as expected.