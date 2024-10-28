# Assetsore - file paths

## bert on 2019-08-18T18:17:24.304Z

Hi,


when I try to show image with large image plugin that is stored in gridfs, I get this message  

“This assetstore does not expose file paths”  

Is problem with girder or with my mongo database?


Bert


---

## David_Manthey on 2019-08-19T12:07:13.000Z

No. Many of the tile tile sources in the large\_image plugin require that images appear as files that can be read “as if from disk”. You can use the “girder mount” command to create a FUSE file system; if you do this, any assetstore can be used with any large\_image tile source.


* David

---

## bert on 2019-08-25T19:01:30.586Z


```
fuse: failed to exec fusermount: No such file or directory
Failed to mount FUSE.  Does the mountpoint ('/girder/mount') exist and is it empty?  Does the user have permission to create FUSE mounts?

```

Hi, I got this error when tried to use girder mount in docker container. The file exists and I am root so I permission to create mounts.


---

## David_Manthey on 2019-08-26T12:09:59.000Z

You have to run docker containers with appropriate permissions to use a user file system inside them. You can do this by adding \-\-privileged to the docker run command. If you are concerned about docker container security and permissions, you can specify a more limited scope of permissions for the docker container (but I’d have to look up the specifics).


---

