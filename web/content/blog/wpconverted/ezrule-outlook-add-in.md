---
title: 'EZRule - Outlook Add-in'
date: Thu, 01 Feb 2007 06:21:25 +0000
draft: false
tags: ['code']
---

**EZRule** A nice way for business people to right click on a new message and make a rule to add them to a newly created folder or add them to an existing folder. Basic Premise Create a context menu, that the user can click, type in a folder name, and have all messages from that person go into the newly created folder. And create a rule so all future emails from that person go into that folder **Steps** 1\. Open Visual Studio 2005 and select the Outlook 2007 Add-in 2. Appreciate the beauty of Aliases using Outlook = Microsoft.Office.Interop.Outlook; 3. Create the form that you will display to use when he/she right clicks on an email item. ![](http://jonshern.files.wordpress.com/2007/02/013107-0759-myfirstoutl2.png) 4. The "ThisAddIn\_Startup" method is where the delegate will be assigned to the Context Menu event

> ```
> this.Application.ItemContextMenuDisplay += new Microsoft.Office.Interop.Outlook.ApplicationEvents\_11\_ItemContextMenuDisplayEventHandler(Application\_ItemContextMenuDisplay);
> ```

5\. In the Context Menu Handler we'll check to see what kind of item we are dealing with.

> ```
> OutlookItem oItem = new OutlookItem(Selection\[1\]); 
> 
> if ((Selection.Count == 1) && (oItem.Class == Outlook.OlObjectClass.olMail))
> ```

```
6\. Next we will put the context menu item on the menu
```

> ```
> commandBarButtonRules = (Office.CommandBarButton)CommandBar.FindControl(Type.Missing, Type.Missing, "EZRule", Type.Missing, Type.Missing); if (commandBarButtonRules == null)
> ```

7\. Now set the style of the menu, instantiate the form event, and assign the click event.

```
// Hook up an event listener to the button 

commandBarButtonRules.Click += new Microsoft.Office.Core.\_CommandBarButtonEvents\_ClickEventHandler(oCBBRules\_Click); 

// Obtain MailItem from OutlookItem.InnerObject oMail = ((Outlook.MailItem)oItem.InnerObject); 

// Pass trusted Application and MailItem in Constructor for RulesDialog 

ezRulesDialog = new EZRuleDialog(this.Application, ((Outlook.MailItem)oItem.InnerObject));
```

The click event just displays the form The form is pretty basic. The create button is really the only button that does anything. Since this is for moving messages to a folder, a folder name is required. **Here is the check folder existence and create new folder methods.**

* * *

```
  private void CreateCustomFolder()
        {

            if(string.IsNullOrEmpty(txtFolder.Text))
            {
                MessageBox.Show("You must provide a folder for this rule.", "EZ Rule", MessageBoxButtons.OK, MessageBoxIcon.Information);
                txtFolder.Focus();
                return;

            }
            Outlook.MAPIFolder inBox = (Outlook.MAPIFolder)
                application.ActiveExplorer().Session.GetDefaultFolder
                (Outlook.OlDefaultFolders.olFolderInbox);
            Outlook.MAPIFolder customFolder = null;
            try
            {
                if (txtParentFolder.Text != String.Empty)
                {
                    folder = inBox.Folders\[txtParentFolder.Text\].Folders.Add(txtFolder.Text,
                    Outlook.OlDefaultFolders.olFolderInbox);
                }
                else
                {
                    folder = inBox.Folders.Add(txtFolder.Text,
                        Outlook.OlDefaultFolders.olFolderInbox);
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("The following error occurred: " + ex.Message);
            }
        }

        private bool DoesFolderExist(string folderName)
        {
            Outlook.MAPIFolder inBox = (Outlook.MAPIFolder)
                application.ActiveExplorer().Session.GetDefaultFolder
                (Outlook.OlDefaultFolders.olFolderInbox);
            try
            {
                application.ActiveExplorer().CurrentFolder = inBox.
                    Folders\[folderName\];
                application.ActiveExplorer().CurrentFolder.Display();
            }
            catch
            {
                return false;
            }
            return true;
        }
```**The Create Rule method is where all of the magic happens**

* * *

```
       private void CreateRule()
        {
            Outlook.Recipient recip;
            Outlook.Rules rules;
            Outlook.Rule rule;

            if (radioButtonDomain.Checked && string.IsNullOrEmpty(txtDomain.Text))
            {
                MessageBox.Show("You must provide a domain for this rule.", "EZ Rule", MessageBoxButtons.OK, MessageBoxIcon.Information);
                txtFolder.Focus();
                return;

            }

            if (radioButtonDomain.Checked && string.IsNullOrEmpty(txtSentFrom.Text))
            {
                MessageBox.Show("You must provide a Sent To for this rule.", "EZ Rule", MessageBoxButtons.OK, MessageBoxIcon.Information);
                txtFolder.Focus();
                return;

            }

            if (string.IsNullOrEmpty(txtRuleName.Text))
            {
                MessageBox.Show("You must provide a Rule Name.", "EZ Rule", MessageBoxButtons.OK, MessageBoxIcon.Information);
                txtFolder.Focus();
                return;

            }

            try
            {
                rules = application.Session.DefaultStore.GetRules();
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.ToString(), "Create EZRule", MessageBoxButtons.OK, MessageBoxIcon.Error);
                this.Close();
                return;
            }
            //Create the rule
            try
            {
                rule = rules.Create(txtRuleName.Text, Outlook.OlRuleType.olRuleReceive);
            }
            catch
            {
                MessageBox.Show("Could not create the rule '" + txtRuleName.Text + "'", "Create Rule", MessageBoxButtons.OK, MessageBoxIcon.Error);
                return;
            }
            //Create conditions by enabling RuleCondition
            try
            {
                if (radioButtonEmail.Checked)
                {
                    Outlook.ToOrFromRuleCondition from = rule.Conditions.From;
                    recip = from.Recipients.Add(txtSentFrom.Text);
                    recip.Resolve();
                    from.Enabled = true;
                }
                else
                {
                    Outlook.ToOrFromRuleCondition domain = rule.Conditions.From;
                    recip = domain.Recipients.Add(txtDomain.Text);
                    recip.Resolve();
                    domain.Enabled = true;
                }

                //Primary task of app is to sort by folder

                Outlook.MoveOrCopyRuleAction move = rule.Actions.MoveToFolder;
                move.Folder = folder;
                move.Enabled = true;
            }
            catch (Exception ex)
            {
                LogMessage("cmdOK\_Click: " + ex.ToString(), EventLogEntryType.Error);
            }

            //Save the rules collection
            try
            {
                this.Hide();
                rules.Save(true);
            }
            catch (Exception ex)
            {
                MessageBox.Show("Could not save the rule '" + txtRuleName.Text + "'", "Create Rule", MessageBoxButtons.OK, MessageBoxIcon.Error);
                LogMessage("Rules.Save: " + ex.ToString(), EventLogEntryType.Error);
                this.Close();
            }

            //Execute the rule
            try
            {
                if (chkRunRule.Checked)
                {
                    Outlook.Explorer expl = application.ActiveExplorer();
                    Outlook.Folder currentFolder = (Outlook.Folder)expl.CurrentFolder;
                    rule.Execute(true, currentFolder, true, Outlook.OlRuleExecuteOption.olRuleExecuteAllMessages);
                }
                this.Close();
            }
            catch (Exception ex)
            {
                LogMessage("Rule.Execute: " + ex.ToString(), EventLogEntryType.Error);
            }

        }
```I think it is worth noting how easy it is to perform this task, with the help of Microsoft's RulesAddin(and a lot of their code)I was able to create this in an evening. Outlook programming has really come along way, and I can't wait to create some more useful addins that really save me time.