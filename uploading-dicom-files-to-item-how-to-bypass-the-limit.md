# Uploading DICOM files to Item: How to bypass the limit?

## LucasGandel on 2018-05-15T12:35:18.615Z

When uploading series of DICOM files into a single item, checking of already existing files misbehave after the 50 first files have been checked. All files after the 50th will be re\-uploaded.


I’m using the following python client command:  

`client.upload(localDICOMFolder, DICOMFolder['_id'], parentType='folder', leafFoldersAsItems=True, reuseExisting=True, blacklist=None, dryRun=False)`


Increasing the DEFAULT\_PAGE\_LIMIT in girder\_client/*init*.py does not help. My guess is that the isFileCurrent() function (called from upload()) should use this variable to specify the request return limit. Changing line 804 of girder\_client/*init*.py to the following does the trick:  

`itemFiles = self.get(path, parameters = {'offset':0, 'limit':DEFAULT_PAGE_LIMIT})`


Not sure this is a bug or me using the upload function in a wrong way. Any idea?  

By the way, is there any better way to define the DEFAULT\_PAGE\_LIMIT variable, other than changing it directly in the *init*.py script?


Thank you very much,  

Lucas


---

## Jonathan_Beezley on 2018-05-15T12:48:09.312Z

You have the right idea. It looks like a bug to me. I think the correct solution is to replace the lines [\_*init\_*.py 839\-840](https://github.com/girder/girder/blob/master/clients/python/girder_client/__init__.py#L839-L840) with



```
itemFiles = self.listFile(itemId)

```

This will iterate through **all** files in the item.


---

## LucasGandel on 2018-05-15T12:53:28.453Z




![](https://discourse.girder.org/user_avatar/discourse.girder.org/jonathan_beezley/48/16_2.png) Jonathan\_Beezley:

> itemFiles \= self.listFile(itemId)



Works well and much cleaner! Thanks a lot!


---

