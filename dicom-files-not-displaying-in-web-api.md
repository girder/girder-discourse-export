# DICOM files not displaying in web API

## TerezaSadilova on 2019-07-15T13:20:24.420Z

Hello,  

I uploaded some testing DICOM files from [https://wiki.cancerimagingarchive.net/display/Public/RIDER\+NEURO\+MRI](https://wiki.cancerimagingarchive.net/display/Public/RIDER+NEURO+MRI), these (900 kB each) are displaying nicely as shown in Girder documentation. However when I uploaded .dcm files created by ndpi to dcm conversion these files (10 MB each) are not displaying. Can you please help me with this issue?  

Thank you.  

Best regards  

Tereza Sadilová


---

## David_Manthey on 2019-07-23T12:10:56.091Z

Do you have a sample file that you can share?


The Girder large\_image plugin can show ndpi files directly, so rather than converting them, you could try that.


– David


---

## TerezaSadilova on 2019-07-23T13:12:03.000Z

Hello,


thank you for your reply. We ended up installing HistomicsTK for browsing ndpi files (with large\_image plugin) and this works great for us. It is possible there was a problem in incorrect conversion of our file. I would have one question though: how large DICOM files is Girder able to display?


Thank you


Best regards


Tereza


út 23\. 7\. 2019 v 14:10 odesílatel David Manthey via Girder [noreply@discourse.girder.org](mailto:noreply@discourse.girder.org) napsal:


---

