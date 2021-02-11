---
title: 'Click Detection in a WPF Listview'
date: Wed, 09 Jul 2008 04:57:56 +0000
draft: false
tags: ['code', 'wpf']
---

Once you have the event wired up, getting the current row only takes a couple lines of code.```

        private void listPictures\_MouseDoubleClick(object sender, MouseButtonEventArgs e)
        {
            Shern.PhotoManager.Common.ImageProperty value = (Shern.PhotoManager.Common.ImageProperty)listPictures.SelectedValue;
            PictureViewer pictureViewer = new PictureViewer(value.FileName);
            pictureViewer.Show();
        }


```In this example ImageProperty is the type that is bound to the ListView control.