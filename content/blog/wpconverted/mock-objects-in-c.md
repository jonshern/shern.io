---
title: 'Mock Objects in C#'
date: Thu, 26 Apr 2007 05:35:44 +0000
draft: false
tags: ['code']
---

From what I have seen there are 3 solutions NMock, RhinoMock and TypeMock. NMock and RhinoMock are opensource and free. TypeMock is free\*, which means they have a community edition that is free and an enterprise edition that you have to pay for. TypeMock is much more robust than the previous two solutions.  I would go with NMock or RhinoMock if you are just starting a TDD solution and you want a mock framework.  If you are adding Mock objects to an existing solution I would go with TypeMock. With RhinoMock and NMock you need interfaces to facilitate the creation of your Mocks.  TypeMock you just need code.  You can create mock objects from methods, concrete classes, and even sealed classes. Mocking works good for telling a test what methods we know will be executed and then verifying those calls are performed. A great example would be You have a data layer that returns datasets. You want to test code that touches the datalayer but you do not want to actually access the database. You would create a mock object like```
DataTable expected = null;
DataTable actual;
MockManager.Init();
Mock dataLayerMock = MockManager.Mock(typeof(DataLayer.Widgets));

DataTable dt = new DataTable();  
//here you could mimic the structure of what the datalayer will return
dataLayerMock.ExpectAndReturn("GetWidgetsThatNeedToBeShipped",dt);
actual = BusinessLayey.Widgets.GetWidgetsThatNeedToBeShipped();
Assert.AreEqual(actual, dt);     
//in this case the business layer just passes the datatable from
//the datalayer to the presentation layer,
//so they would be equal, and the test would pass.

MockManager.Verify();

```