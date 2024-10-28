# girder_client 3.0.0a6 and girder 2.5, KeyError: 'release' on upload

## Charles on 2019-04-04T16:20:54.270Z

Hello,


I try to use the python girder\_client version 3\.0\.0a6\.dev15 to communicate with a girder server version 2\.5\. My version of python is 3\.7\.


I have an error when trying to upload a file on the server, using either **uploadFileToFolder** or **uploadFileToItem** method.


The error is the following:



```
    9 || Traceback (most recent call last):
    8 ||   File "test.py", line 7, in <module> 
    7 ||     fileGirder = client.uploadFileToFolder(...)
    6 ||   File ".../girder_client-3.0.0a6.dev15-py3.7.egg/girder_client/__init__.py", line 1006, in uploadFileToFolder
    5 ||     progressCallback)
    4 ||   File ".../girder_client-3.0.0a6.dev15-py3.7.egg/girder_client/__init__.py", line 957, in uploadStreamToFolder
    3 ||     if size <= self.MAX_CHUNK_SIZE and self.getServerVersion() >= ['2', '3']:                                                                                                                                 
    2 ||   File ".../girder_client-3.0.0a6.dev15-py3.7.egg/girder_client/__init__.py", line 382, in getServerVersion
    1 ||     self._serverVersion = self.get('system/version')['release'].split('.', 3)                                                                                                                                 
  18  || KeyError: 'release'

```

In my case, the result of `self.get('system/version')`, line 1 is:



```
 {'SHA': '582567eb86ec6518281c758b3870c0bcac1afd82', 'apiVersion': '2.5.0', 'date': '2019-03-01T17|28| 09+0000', 'git': True, 'serverStartDate': '2019-03-12T16:50:59.381648+00:00', 'shortSHA': '582567e'}

```

It does not contains the field `release` as required.


In order to reproduce, you can try this little python example:



```
import girder_client as gc

client = gc.GirderClient(apiUrl = 'http://127.0.0.1:8080/api/v1')
client.authenticate(apiKey = '<apiKey>')                                                                                                                                         
temp_folderId = '<rootFolderId>'                                                                                                                                                                       
folderGirder = client.loadOrCreateFolder('newFolder', temp_folderId, 'folder')                                                                                                                                  
fileGirder = client.uploadFileToFolder(folderGirder['_id'], '<fileToUpload>')

```

Is girder\_client 3\.0 not compatible with girder 2\.5 ?


---

## Charles on 2019-04-05T08:11:29.372Z

Remarque:


Replacing



```
self._serverVersion = self.get('system/version')['release'].split('.', 3)

```

by



```
self._serverVersion = self.get('system/version')['apiVersion'].split('.', 3)

```

solve the issue in my case. Not sure it would work with newer girder version though.


---

## Jonathan_Beezley on 2019-04-05T13:27:28.932Z

Thanks for reporting. I submitted a fix at <https://github.com/girder/girder/pull/2976> that should go in before the final release.


---

