# Downloading Region of Interest along with the annotations

## RazerEdge64 on 2021-05-01T07:01:44.960Z

I have looked for plugins to download a ROI from the WSI along with the annotations. I have failed to find any such plugin.  

Is it even possible to do such a thing?  

(I need the ROI from the image to do testing for my AI algorithm)  

If the ROI could be downloaded as a jpeg image that would be great for me.


[![image](https://discourse.girder.org/uploads/default/optimized/1X/7ae66560149e0721e4ee435b6f75922ec82d3ecb_2_690x365.jpeg)
image1637×867 790 KB](https://discourse.girder.org/uploads/default/original/1X/7ae66560149e0721e4ee435b6f75922ec82d3ecb.jpeg "image")


Please guide me for any alternatives if the direct method is not possible!


Sorry for bombarding you with questions every now and then ![:slightly_smiling_face:](https://discourse.girder.org/images/emoji/twitter/slightly_smiling_face.png?v=9 ":slightly_smiling_face:")


---

## David_Manthey on 2021-05-03T17:16:02.234Z

You can download a region using the `GET /item/{itemId}/tiles/region` endpoint. This lets you specify the area of the region, the output resolution (so you can downsample it, if desired), the encoding (JPEG, PNG, TIFF), and many other parameters.


From the UI, the Download View and Download Area options use this to download a specific region.


\- David


---

## RazerEdge64 on 2021-05-05T08:27:28.113Z

Thank you for the reply David!  

I assume not but, is there a way to catch those annotations on the downloaded region aswell?


---

## David_Manthey on 2021-05-05T12:58:01.033Z

You can download individual annotation records and limit each one to your specified region via the `GET /annotation/{id}` endpoint. This would get you a json object for each annotation and only include elements whose bounding box overlaps the specified region (meaning that you might have a polygon annotation which isn’t in your region but is very close to it). You’d have to enumerate all the annotations for the item and query each one separately.


If you wanted the annotations rendered on the region, this would have to be done on the client – there isn’t any annotation rendering on the server side.


---

