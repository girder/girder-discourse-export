# Anonymization/Pseudonymization

## vickyapril02 on 2021-08-06T12:04:03.072Z

Hi,


Thanks for this support page. Our team is really enjoying the features of Girder.  

We just have a question , In our project we need all our data’s to be anonymized/ pseudonymized as per our protocol .  

Is there any plugins available in Girder or any other method to do Anonymization of data’s.


Any leads would be a great help for our team


Thanks and Regards  

Vicky


---

## David_Manthey on 2021-08-06T12:11:31.902Z

There is a project that has such a plugin here: [GitHub \- DigitalSlideArchive/DSA\-WSI\-DeID: A workflow for redacting PHI from whole slide images (WSI) based on the Digital Slide Archive.](https://github.com/DigitalSlideArchive/DSA-WSI-DeID) that allows redacting metadata and some images. There is ongoing work to increase the configurability of that plugin (see the “more\-configure\-options” branch). If that plugin is added and appropriately configured, WSI files within a few formats (Aperio, Hamamatsu, Philips TIFF) can be redacted if they are placed in the appropriate folder either by importing them with a manifest file as specified in the WSI\-DeID project or by just moving them into the appropriate folder.


* David

---

## vickyapril02 on 2021-08-06T12:23:37.330Z

Thanks David for the super fast positive response


---

