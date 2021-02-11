---
title: 'Enterprise Library 4 Summary'
date: Mon, 21 Jul 2008 04:23:17 +0000
draft: true
tags: ['code']
---

A high level summary of the application blocks offered on [codeplex](http://www.codeplex.com/entlib/Release/ProjectReleases.aspx?ReleaseId=13498)

**The Caching Application Block**

Allows for the creation of on caching layer either using an isolated store or database. Things such as expirations and scavending can be easily configured.

**The Cryptography Application Block**

Used to standardize the methods that are used to encrypt data,  get hashes from data, and compare hash values.

**The Data Access Application Block**

Used to encapsulate common database functionality like connection management, and caching parameters.  This one greatly simplifies the use of databases and allows for a nice factory to create database connections.

**Exception Handling Application Block**

Allows for the definition of exception handling policies, so an exception handling policy like we need to show less info in this build can be set by an administrator.  Functionality around logging, and other ways to write out exceptions can be utilized.

**Logging Application Block**

Simplifies the use of common logging functionality like writing to the event log, sending an email mesage, writing to the database, message queue, text file, and other custom connections.

I have used log4net and I have come to like there use of logging levels and appenders(the way that message are appended to an io channel)

**Policy Injection Application Block**

Allows the definition of policies that can be applied to classes, methods, properties, etc.

Usefull for:

*   Logging Method Invocation and Property Access
*   Handling Exceptions in a Structured Manner
*   Validating Parameter Values
*   Caching Method Results and Property Values
*   Authorizing Method and Property Requests
*   Measuring Target Method Performance

**Security Application Block**

Handles common security related tasks like authorization and credential caching.

1\. Define roles

2\. Define rules that determine what roles can do and what they cannot.  
    There is a tool that allows for easy creation of these rules.  
    <authorizationProviders>  
      <add type="Microsoft.Practices.EnterpriseLibrary.Security.AuthorizationRuleProvider, Microsoft.Practices.EnterpriseLibrary.Security, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"

        name="RuleProvider">  
        <rules>  
          <add expression="R:Employee OR Manager" name="ViewMyTimeSheet" />  
          <add expression="R:Manager" name="ViewAllTimeSheets" />    
        </rules>  
      </add>  
    </authorizationProviders>

In this example three rules have been defined. 

*   An Employee or manager can view my time sheet.
*   Only a manager can view all timesheets

3\. Create the Principal in code when the user logs in(simple version with no password)  
    UserPrincipal = new GenericPrincipal(new GenericIdentity("pete"), new string\[\] { "Employee" });  
    //create a user named pete with a role of Employee

4\. When a specific function is going to happen check to see if the user is authorized.  
    IAuthorizationProvider ruleProvider = new AuthorizationFactory.GetAuthorizationProvider("RuleProvider");  
    ruleProvider.Authorize(UserPrincipal, authorizationRule);  
    //The authorization rule maps to the rules defined in the config. 

**Unity Application Block**

Dependency injection framework.  Helps when implementing TDD with Mock Objects.

**Validation Application Block**

Helps developers write validation that can wired from Business Objects to UI. These validation rules can be driven through code or configuration.