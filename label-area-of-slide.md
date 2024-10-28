# label area of slide

## vigi04 on 2021-06-11T07:41:59.576Z

Hello  

Is it possible to get the label image of the whole slide image as another “large\_image”. I would like to see the label image along with the actual whole slide image. By “label image” , i dont mean the image after annotations. I mean the image scanned by the scanner which contains the text written on the scanned slides. This information is also present in the whole slide image. But, the girder doesnt read this image.


Best  

Vignesh


---

## David_Manthey on 2021-06-11T12:23:22.867Z

Hi Vignesh,


What context are you in where you want to see the label image? We show it on the Girder item page. It can optional be shown when listing items in folders via a setting in the large\_image Girder plugin. Programmatically, when you open a tile source in python via something like (`ts = large_image.open(<file path>)`, you can then get the label image via `ts.getAssociatedImage('label')`, which returns a tuple of the image and its mime type. This is usually a jpeg or png. If you needed to, you could save that image to a file and then use large\_image\_converter either from the command line or programmatically to make it a pyramidal tiff.


* David

---

## vigi04 on 2021-06-11T13:42:32.822Z

Thanks a lot. I could get those images from the girder plugin in options.  

To do it programatically, I realised that I would need to download the file first in order to read it via “large\_image.open”.


---

