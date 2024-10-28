# Uploading a file using python requests

## oleks_korshak on 2020-02-18T08:09:27.799Z

I am using the code below to try to upload a file to my girder instance using python3\. I keep getting a response code saying “Chunk is smaller than the minimum size.” which implies that the data is not being read by Girder. The API documentation says: “The data for the chunk should be sent as the body of the request using an appropriate content\-type and with the other parameters as part of the query string.” What is the proper way to make this work?



> ```
> def read_in_chunks(file_object, chunk_size=1024):
>     while True:
>         data = file_object.read(chunk_size)
>         if not data:
>             break
>         yield data
> 
> response = requests.post(
>         url = serverAddress + 'api/v1/file', 
>         params = {'parentId': parentId, 'name': name, 'parentType': parentType, 'size': filesize, 'mimeType': 'application/octet-stream'},
>         headers = {'Girder-Token': accessToken}
>     )
> uploadid = response.json()['_id']
> chunk_size= 1024
> offset = 0
> with open(path, 'rb') as filehandle:
>         for piece in read_in_chunks(filehandle, chunk_size=chunk_size):
>             response = requests.post(
>                 url = serverAddress + 'api/v1/file/chunk', 
>                 params = {'uploadId': uploadid, 'offset': offset},
>                 headers = {'Girder-Token': accessToken},
>                 data = piece
>             )
>             offset += chunk_size
> 
> ```


---

## Zach_Mullen on 2020-02-18T14:38:06.706Z

Hello,


The minimum size you can send in a single HTTP request, by default, is 5MB. You are attempting to send 1KB chunks, which will end up being extremely slow due to the overhead of each request. I’d recommend using 32MB or 64MB chunks, which is what most Girder clients use.


Since you’re using Python anyway, the `girder-client` library can handle a lot of this for you: [https://girder.readthedocs.io/en/stable/python\-client.html](https://girder.readthedocs.io/en/stable/python-client.html)


It has both a command line interface for uploading, as well as a Python library for custom interaction with a Girder REST API.


Thanks,


\-Zach


---

## oleks_korshak on 2020-02-18T18:11:31.949Z

Thanks for the info Zach!  

I’ll try out using bigger chunks and look into the python girder\-client library.


Oleks


---

