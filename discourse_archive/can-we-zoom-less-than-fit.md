# Can we zoom less than fit?

## RazerEdge64 on 2021-04-19T08:04:22.719Z

Is there a way to zoom less than fit?  

I am currently using a very cheeky way to do it (just by increasing the imageHeight and imageWidth).  

in *imageView.js*  

this.imageWidth \= this.viewer.maxBounds().right*4;  

this.imageHeight \= this.viewer.maxBounds().bottom*4;  

But, the viewer is sticking to the top left whenever we zoom in like this.  

[![image](https://discourse.girder.org/uploads/default/optimized/1X/0e8ca240531e618099b3d91e3b1ddf9ee5bc53a9_2_690x319.png)image1638×758 80\.1 KB](https://discourse.girder.org/uploads/default/original/1X/0e8ca240531e618099b3d91e3b1ddf9ee5bc53a9.png "image")  

I am able to pan it later but, it is getting restricted near the bottom right area.  

What can i do to avoid this?  

Basically, i need to be able to show even level 0,1,2 tiles before jumping to Fit . And the tiles loaded at fit vary from image to image.


---

## RazerEdge64 on 2021-04-19T08:05:57.180Z

Initially,  

[![image](https://discourse.girder.org/uploads/default/optimized/1X/96f673e86c459c908f465eee017a4f2c770ab24b_2_690x346.jpeg)image1637×823 223 KB](https://discourse.girder.org/uploads/default/original/1X/96f673e86c459c908f465eee017a4f2c770ab24b.jpeg "image")  

Here, level 2 tiles are being loaded. I would want to show even level 1 and level 0 which require zooming out.  

Is there any way I can do it?


---

## David_Manthey on 2021-04-19T12:10:21.538Z

There are a couple of parameters you can adjust to do this fairly easily. See [HistomicsUI/ImageView.js at 4f6df240e127f1b1725ef2556798f299126dd798 · DigitalSlideArchive/HistomicsUI · GitHub](https://github.com/DigitalSlideArchive/HistomicsUI/blob/4f6df240e127f1b1725ef2556798f299126dd798/histomicsui/web_client/views/body/ImageView.js#L198) – the `extraPanWidth` and `extraPanHeight` are used to change the bounds of the viewer, which is probably what you want. You can set the bounds more explicitly if that doesn’t give enough control.


---

## RazerEdge64 on 2021-04-20T06:15:32.502Z

Following your advice worked!  

I made `extraPanWidth` \& `extraPanHeight` as 4\.  

[![image](https://discourse.girder.org/uploads/default/optimized/1X/e4911149dd72fe12ab6a39635531fb3936d0bdcd_2_690x315.jpeg)image1636×748 73\.8 KB](https://discourse.girder.org/uploads/default/original/1X/e4911149dd72fe12ab6a39635531fb3936d0bdcd.jpeg "image")  

Thank you [@David\_Manthey](/u/david_manthey) !


---

