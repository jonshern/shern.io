---
title: 'Sql Server Compact - Access to the database file is not allowed.'
date: Tue, 21 Oct 2008 00:30:40 +0000
draft: false
tags: ['code']
---

Lately I have been playing with some small client apps, just kicking the tires on a few of the new microsoft technologies. For the underlying database I chose Sql Server Compact Edition. Since I am using Linq to Entities, stored procedure support is not necessary. I wrote a few tests against the database and found that the database was not being deployed along with the tests.  Here is app config entry |DataDirectory|DataLayerPhotoStore.sdf. I copied that from the Common Library, when the test was ran the entry parsed into  TestResultsJon\_JON-PC 2008-10-20 18\_57\_13OutDataLayerPhotoStore.sdf The first error was the obvious file not found error. My first instinct was just to try and copy it to a common directory. I received the exception below. System.Data.EntityException: The underlying provider failed on Open. --->  System.Data.SqlServerCe.SqlCeException: Access to the database file is not allowed. \[ File name = C:UsersJonDocumentsPhotoStore.sdf \]. **Resolutions** 1. Turning off UAC. 2. Point the tests app.config to the database file in the corresponding app directory. 3. Run Visual Studio as administrator. 4. Create a build task to deploy the database in the test directory (TestResultsJon\_JON-PC 2008-10-20 18\_57\_13Out) I think the reason this is happening is because of the LUA model of security. When I copy the data file into  directory like Users/JonShern/Documents by default the test process is running as a least privileged user, so it not able to write to this file.  Admittedely my understanding of the inner working of security are lacking, seems like it just ends up to be the guess and test method of problem solving:( I think 1,2 and 3 seem a little hackish to me. But man to write a build task for a little app is just overkill. Here is a code sample that someone is using for deployments.

SecurityIdentifier sid = new SecurityIdentifier( WellKnownSidType.BuiltinUsersSid , null );

FileSecurity dbSec = File.GetAccessControl( dbFile );

dbSec.AddAccessRule( new FileSystemAccessRule( sid , FileSystemRights.Write, AccessControlType.Allow ) );

File.SetAccessControl( dbFile , dbSec );

**References** [http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=1892312&SiteID=1](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=1892312&SiteID=1) [http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=3455426&SiteID=1](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=3455426&SiteID=1)