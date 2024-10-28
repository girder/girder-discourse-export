# Beginner's questions

## valery on 2021-12-02T16:17:04.740Z

Dear Girder expert.


# Context


I found Girder quite impressive and easy to set up. I would like to use it to deploy image processing methods to in a research lab. I’m investigating the use of girder for two applications:


* short job (\<10s typically applying ML weights) using a simple plugging
* long job (\> 1h\-6h ) using the job system described in girder\-worker.


For both jobs, the scenario is the following, the user upload a dataset (a Nifti file) as input. The job run (typically a threshold using sitk) and saved the corresponding label to the database.


So far, I’ve succeed to :


* install girder,
* use the Python Client and Girder CLI to create the database \& upload or download data
* create a plugin that triggers when a file is uploaded.


# Question 1


Disclaim, most of the concept of Girder are far away from my background


Using the plugin mecanism, I haven’t been able to read the uploaded nifti file. For instance:



```
def handler(event):
    # get file
    file = event.info['file']  
    # get path
    path=file['path']
    # get itemId
    itemId=file['itemId']
    # create Item
    item = Item().load(file['itemId'], force=True)
    # download file
    download_file=File().download( file, offset=0, headers=True, endByte=None, contentDisposition=None, extraParameters=None)
    
that return <function File.download.<locals>.downloadGenerator at 0x7fe0a1f5b8b0> 


```

Could you comment what is a downloadGenerator and how to access to the data for instance using sitk. Or what would you advise for I/O in regard to the girder database inside a plugin.


# Question 2


I spend time reading : [girder\_worker/TRANSITION.md at master · girder/girder\_worker · GitHub](https://github.com/girder/girder_worker/blob/master/TRANSITION.md) and [girder\_worker/HACKING.md at master · girder/girder\_worker · GitHub](https://github.com/girder/girder_worker/blob/master/HACKING.md)


While I succeed to get the concept associated with the creation of “custom Celery task for Girder Worker” , the transition between Girder and the Girder worker is not easy to catch. In case, if existing, could you recommend a complete example available on the web that include the link between i) the detection of an event in a plugin, ii) the creation of girder job and the task, iii) the I/O part (GirderFileId/GirderUploadToItem).


Thanks in advance,


Best,  

Valéry


---

## valery on 2022-01-04T15:04:41.529Z

Hi,


I took me a while but I finally succeed to access to the Nifti file in a plugin. I’m now struggling with the two following issues:


## Question 1


How can I get the full folder ID of the document in a plugin? In the API via ([http://127\.0\.0\.1:8081/api/v1\#!/folder/folder\_getFolder](http://127.0.0.1:8081/api/v1#!/folder/folder_getFolder)) it is super easy but in a plugin , the command “def getFolder(self, folder)” is not accessible.


Is there another way to get the parent folder of an item or a file that return the complete dictionary ? Having the folder id via the item is straightforward but the complete dictionary is needed for instance for creating a new item in the same folder.


## Question 2


Could you please briefly comment if it is possible to upload a nifti file in a similar fashion [s3fs\_nifti.ipynb · GitHub](https://gist.github.com/arokem/423d915e157b659d37f4aded2747d2b3)


My difficulty is to attach/upload the actual data to a new file in an item for instance.


Thanks in advance,


Valéry



```

def handler(event):

    # get file
    file = event.info['file']  
    
    # get item
    item = Item().load(file['itemId'], level=AccessType.WRITE, force=True)
    
    #read the nifti
    # see https://gist.github.com/arokem/423d915e157b659d37f4aded2747d2b3 
    with File().open(file) as f:
      rr = f.read()
      bb = BytesIO(rr)
      fh = FileHolder(fileobj=bb)
      nib_image = nib.Nifti1Image.from_file_map({'header': fh, 'image': fh})
    
    # let's assume some operation on the nifti file
    new_nib_image=nib_image

    # prepare the nifti
    bio = BytesIO()
    file_map = new_nib_image.make_file_map({'image': bio, 'header': bio})
    new_nib_image.to_file_map(file_map)
    data = bio.getvalue()
    
    # upload the nifti
    # QUESTION how to upload the file to the same location or to a new location ?
    
    creator=event.info['currentUser']
    size=File().updateSize(file)    
    
    #with open(filename, 'rb') as f:
    #  Upload().uploadFromFile(f, size, filename='lili', 'item', parentItem=item, user=creator)
                

```

---

## David_Manthey on 2022-01-04T17:48:52.769Z

If you have a folder ID, you can load the folder document like so:



```
from girder.models.folder import Folder

folderDocument = Folder().load(folderID, ...)   # pass parameters to specify what access you need (e.g., the user with permission to access it, or force=True to always load it).

```

`folderID` can be from an item (e.g., `item['folderId']`)


Your commented out code from `uploadFromFile` is nearly right – you have to specify the parent as `parent=<item or folder document>` and `parentModel="item"` or `parentModel="folder"` as appropriate.


– David


---

## valery on 2022-01-05T13:16:24.957Z

Hi David,


Thanks a lot for your help. The load method was super helpful.  

Regarding the second point , I wasn’t able to use the uploadFromFile method instead I created an empty file and getting the absolute path, I write the data in it. I add the code below. In case , I have two additional small questions:


1. Doing such steps, I created an infinite loop: as my function is triggered by an event: ‘data.process’ , the upload triggered a second upload , third upload … I did a quick hack by if/else condition based on the name of the input and saving files. Is there some mechanisms to filter/remove event like ‘data.process’ in the upload function ?
2. My understanding of createFolder is ok, createItem is ok but I do not understand the logic of createFile: “Create a new file record in the database”. For instance if I wish to create a new file in an item.



```
new_file=File().createFile(creator=creator, item=doc, name="new_file", size=size_of_the_file)
ERROR:createFile() missing 1 required positional argument: 'assetstore

```

The definition mentioned “assetstore: The assetstore this file is stored in.” So I need to add the assetstore in the call of the function. However it is unclear to me where to find this value as I haven’t yet create the file and I do not have yet a specific location . I missed something. In which context , this function should be used so ?


Thanks a lot in advance  

Valéry



```
def girder_nifti_write(nib_img, name , creator, item):
    
    #IO stuff
    bio = BytesIO()
    file_map = nib_img.make_file_map({'image': bio, 'header': bio})
    nib_img.to_file_map(file_map)
    data = bio.getvalue()

    print(bio.getbuffer().nbytes)

    # create a empty file
    upload_created=Upload().createUpload( user=creator, name=name, parentType='item', parent=item, size=bio.getbuffer().nbytes)
    new_file_from_upload=Upload().finalizeUpload(upload_created)
    
    # check if it is a file
    print(File().validate(new_file_from_upload))
    
    # get the path 
    local_path_new_file=File().getLocalFilePath(new_file_from_upload)
    
    # write the data
    with open(local_path_new_file, 'wb') as f: 
        f.write(bio.getbuffer())

    bio.close()  

```

---

