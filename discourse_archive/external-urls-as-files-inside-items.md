# External URLs as Files inside Items

## jfernandez on 2021-03-17T10:23:51.059Z

Hi!


I have a doubt about including external URLs as Files inside a Girder Item, which is a [feature described in the “Files” section of the User Guide](https://girder.readthedocs.io/en/latest/user-guide.html?highlight=external#files).


I would like to try this feature in a Girder 3\.1\.3 instance I have installed recently, but in the Item page I just see the icon to upload files, and the Upload Files modal only prompts the user to browse or drag files.


Is this “Files as External URLs” feature really implemented? If so, how can I use it?


Thanks!


---

## David_Manthey on 2021-03-17T12:06:45.704Z

There may not be a way in the UI to create a URL reference file. You can do so via the `POST file` endpoint, though. You’ll need to supply the folder or item id for the file destination, a name, and a url (the linkUrl parameter). There are definitely limitations on what you can do with a URL reference file, but the basic functionality works.


---

## jfernandez on 2021-03-17T12:14:53.843Z

Oops, I did not realise that the feature might be available through the API… I have just tried it and it worked exactly as I was expecting to. Thanks again for the answer!


---

