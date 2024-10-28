# HistomicsTK asseststore and Collections

## sfarag on 2020-08-07T23:16:44.256Z

Two questions related to asseststore as a filesystem:


1. Let’s say I downloaded the entire TCGA slides locally, can I just then create a new assestore with the name TCGA and point to the path where it is locally stored, without the need to upload the slides?
2. Similar question but for Collections do I need to upload them or I can just refer to their folder path?


Best regards,


Sherif


---

## David_Manthey on 2020-08-10T13:04:53.567Z

Yes. From the Girder Admin Console, you an go to a filesystem assetstore and use the Import button to have Girder index files that are already on an accessible file system. When you use import, it will ask where you want them to appear in Girder’s hierarchy. You can choose a Collection for this location (or a User or any folder).


Import just references the existing files. Upload actually transfers them to a Girder controlled location.


* David

---

## sfarag on 2020-08-13T18:46:13.097Z

Thanks David, that was a big help indeed.


Best,


Sherif


---

