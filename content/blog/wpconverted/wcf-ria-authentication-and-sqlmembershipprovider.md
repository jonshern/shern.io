---
title: 'WCF Ria Authentication and SqlMembershipProvider'
date: Thu, 03 Dec 2009 06:40:02 +0000
draft: false
tags: ['code']
---

Getting the Authentication working is a snap, if you remember how to specify the connection string for the SqlMembershipProvider.

I had forgotten.

So it was back to Asp.net 2.0 even though I have an Application built with Silverlight 4.0.

Since WCF RIA Authentication uses the membership provider, and now the web.config is greatly simplified.

The default membership stuff is not in there anymore.

If you are using the SqlMembershipProvider don't forget to add the provider configuration in your web.config.

After you run aspnet\_regsql of courseJ

<providers\>

<add name\="PracticeModelUsers" type\="System.Web.Security.SqlRoleProvider, System.Web, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" applicationName\="PracticeManager" connectionStringName\="PracticeModel" />

</providers\>

</roleManager\>

<membership defaultProvider\="PracticeManagerUserProvider"\>

<providers\>

<add name\="PracticeManagerUserProvider" type\="System.Web.Security.SqlMembershipProvider, System.Web, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" applicationName\="PracticeManager" connectionStringName\="PracticeModel" enablePasswordReset\="false" enablePasswordRetrieval\="false" passwordFormat\="Clear" requiresQuestionAndAnswer\="false" requiresUniqueEmail\="false" />

</providers\>

</membership\>

<profile\>

<properties\>

<add name\="FriendlyName" />

</properties\>

<providers\>

<add name\="PracticeProfileProvider" type\="System.Web.Profile.SqlProfileProvider, System.Web, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" applicationName\="Practice Manager" connectionStringName\="PracticeModel" />

</providers\>

</profile\>