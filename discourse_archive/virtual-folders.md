# Virtual Folders

## vickyapril02 on 2021-10-12T14:22:46.438Z

Hello,


Recently we found a plugin (girder\-virtual\-folders), we installed the plugin but don’t know how to configure or make use of it.  

Any documentation or examples would be a great help.


Thanks and Regards  

Vicky


---

## David_Manthey on 2021-10-12T15:24:51.636Z

The virtual folders plugin modifies the POST folder and PUT folder/{id} endpoints to allow creating virtual folders. These take a mongo query. Any item matching such a query appears as if it is in that folder. A query requires knowledge of the internal database fields. For instance, something like `{"name": {"$regex": {"^abc*.tiff$"}}}` would select all items that whose names start with abc and end in tiff. One virtue to the virtual folder is that its apparent contents are determined dynamically, so creating, renaming, or removing items will affect what the folder appears to contain.


The plugin isn’t widely used, so it may not have the same level of support and testing as better documented ones.


---

## vickyapril02 on 2021-10-12T15:26:56.439Z

Thanks ![:+1:](https://discourse.girder.org/images/emoji/twitter/+1.png?v=10 ":+1:")


---

