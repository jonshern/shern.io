---
title: 'WCF RIA and Server and Client Side Authentication'
date: Sun, 06 Dec 2009 17:27:12 +0000
draft: false
tags: ['code']
---

Works with Silverlight 4.0 beta 1, Visual Studio 2010, and the Silverlight Business Application Template

   
In WCF Ria you can decorate your service methods with the

\[RequiresAuthentication\] attribute to denote the user must be authenticated.

This comes from the System.Web.DomainServices namespace.

This same functionality is not available on the client side.

One method is to pop the login form when a user navigates to a restricted page.

I am not using roles, but you could just as easily use IsInRole(string role) instead.

```
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            LoginUI.LoginRegistrationWindow loginWindow = new LoginUI.LoginRegistrationWindow();

            if (!WebContext.Current.User.IsAuthenticated)
            {
                loginWindow.Show();
            }

            serviceClient.GetTasksAsync(WebContext.Current.User.Name)
        }


```

The user can still see that the page exists.

In order to hide the page you can use the events from WebContext.Current.Authentication

```
        public MainPage()
        {
            InitializeComponent();
            
            
            this.loginContainer.Child = new LoginStatus();

            WebContext.Current.Authentication.LoggedIn += new System.EventHandler(Authentication\_LoggedIn);
            WebContext.Current.Authentication.LoggedOut += new System.EventHandler(Authentication\_LoggedOut);

            ManageMenu();
        }





        void Authentication\_LoggedOut(object sender, System.Windows.Ria.ApplicationServices.AuthenticationEventArgs e)
        {
            ManageMenu();
        }

        void Authentication\_LoggedIn(object sender, System.Windows.Ria.ApplicationServices.AuthenticationEventArgs e)
        {
            ManageMenu();
        }



        private void ManageMenu()
        {
            if (!WebContext.Current.User.IsAuthenticated)
            {
                dashboardDivider.Visibility = System.Windows.Visibility.Collapsed;
                dashboardLink.Visibility = System.Windows.Visibility.Collapsed;
            }
            else
            {
                dashboardDivider.Visibility = System.Windows.Visibility.Visible;
                dashboardLink.Visibility = System.Windows.Visibility.Visible;
            }
        }




```