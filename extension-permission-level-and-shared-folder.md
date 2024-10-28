# Extension Permission Level and Shared Folder

## HannoGao on 2019-04-09T13:52:47.810Z

I am currently evaluation girder. I have different use cases, whereas I am not sure, if I can realise them with girder:


1. Permission Level


The current system has only this AccessTypes:



> NONE \= \-1  
> 
> READ \= 0  
> 
> WRITE \= 1  
> 
> ADMIN \= 2  
> 
> SITE\_ADMIN \= 100


I would need an additional access type between READ and WRITE, which allows users to read files and to upload files, but not to delete files.


Is there an easy solution to extend AcessTypes?


2. Shared Folders


I would like to share folders between collections. The functionality to copy folders exists: The files are not copied, only the metadata information?  

So is the best way to go, to create a plugin that links the folders and whenever some metadata changes in a linked folder, the changes are updated to all linked folders?


---

## HannoGao on 2019-04-16T13:37:26.489Z

For the Permission Level, I see three options:


1. I change the AcessType class, by adding additional access types, this would involve changing/adapting core code everywhere.
2. I create a plugin, listening to events e.g. regarding delete requests and preventing such requests in specific cases.
3. I create a upload plugin, allowing users with READ Access to upload Data within the plugin.


Since option 1\. would involve merging a lot of changes, for future girder releases, I would prefer option 2 or 3\. Are there other options?  

Any idea/hint/suggestion would be appreciated.


---

