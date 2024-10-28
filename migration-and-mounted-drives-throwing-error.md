# Migration and mounted drives throwing error

## rbaustin on 2020-02-06T01:16:52.116Z

I’m having a really weird error, and I’m not sure what steps to try next to solve it.


I’m migrating a Girder 2 instance to Girder 3, and at the same time, upgrading the Python version and moving the database over to MongoDB Atlas. So far, I’ve succeeded in deploying the Girder 3 instance on a new machine with Python 3\. The instance worked fine, as expected. I’ve also migrated the database, and after pointing the Girder instance to that database, it also worked fine.


However, everything seems to work fine except resources that are being served from the assetstores. HTTP errors occur for these routes:


* /api/v1/item/5dd5d09aa0c5fdd5e7556200/tiles/thumbnail
* /api/v1/item/5dd5d09aa0c5fdd5e7556200/tiles


The error returned is:



```
{
  message: "An unexpected error occurred on the server."
  type: "internal"
  uid: "04410cc8-f799-4ef7-b2b7-1a18cce6b325"
}

```

And when I open the girder error logs, I see:



```
[2020-02-05 00:14:13,848] ERROR: 500 Error
Traceback (most recent call last):
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder/api/rest.py", line 630, in endpointDecorator*
    val = fun(self, path, params)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder/api/rest.py", line 1191, in GET
    return self.handleRoute('GET', path, params)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder/api/rest.py", line 947, in handleRoute
    val = handler(**kwargs)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder/api/rest.py", line 401, in wrapped
    return fun(*args, **kwargs)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder_large_image/rest/tiles.py", line 252, in getTilesInfo
    return self._getTilesInfo(item, params)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder_large_image/rest/tiles.py", line 210, in _getTilesInfo
    return self.imageItemModel.getMetadata(item, **imageArgs)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder_large_image/models/image_item.py", line 173, in getMetadata
    tileSource = self._loadTileSource(item, **kwargs)
  File "/home/ubuntu/girder_env/local/lib/python3.6/site-packages/girder_large_image/models/image_item.py", line 162, in _loadTileSource
    tileSource = girder_tilesource.AvailableGirderTileSources[sourceName](item, **kwargs)
KeyError: 'svs'
Additional info:
  Request URL: GET [domain]/api/v1/item/5dd5d09aa0c5fdd5e7556200/tiles
  Query string:
  Remote IP: 127.0.0.1
  Request UID: 0751e470-5df1-478f-8e70-475a92484ddc

```

One thing that is implemented the same across both instances, but is slightly different than a normal Girder instance is that we are using a file mounting tool to sync files between the server and our Wasabi file storage. The tool is called [Goofys](https://github.com/kahing/goofys/), but as far as I know, it is set up exactly the same on the Girder V2 instance as the Girder V3\. And when I go to the same path in the server as the ones listed in the database, the files are there.


---

## David_Manthey on 2020-02-06T14:06:49.000Z

As part of the move to Girder 3 refactor, large\_image renamed the “svs” tile source to “openslide” (since it opens more than svs files). You need to do a one time database update to fix this. We should add this to a migration document or automate it.


Specifically, on a standard mongo instance, you’d run:


mongo girder


to get access to the girder database. Then in the mongo shell, run


db.item.updateMany({“largeImage.sourceName”: “svs”}, {$set: {“largeImage.sourceName”: “openslide”}})


---

## rbaustin on 2020-02-07T19:16:48.412Z

Thanks [@David\_Manthey](/u/david_manthey).


I’ve run the DB update command, and the previous error seems to be fixed, but I’m still not able to get the tiles request to work.



```
{
  identifier: "girder.utility.assetstore.file-path-not-available",
  message: "This assetstore does not expose file paths",
  type: "girder"
}

```

I should mention that I’ve also used `girder mount` to set up the mounted path, and the files it’s trying to access to exist at the specified path.


---

## David_Manthey on 2020-02-07T19:29:49.985Z

Can you download the file associated with the image from Girder UI? If so, then I’m puzzled. If not, then I think we can examine the database records to hunt down the issue.


---

## rbaustin on 2020-02-07T19:54:43.252Z

Yeah, that’s also one weird thing. When trying to download that item, I get:



```
{
    "identifier": "girder.utility.filesystem_assetstore_adapter.file-does-not-exist",
    "message": "File /home/ubuntu/studies/rsync-mirror/19-600 Crinetics/19-600 G.1- 01.mrxs does not exist.",
    "type": "girder"
}

```

But the file does exist there. I can navigate to the file and explore details about it through the terminal. And in this case, we are back to using Goofys for the mount, but the files are treated as local files. Not sure if that is a big issue.


---

## David_Manthey on 2020-02-07T20:07:21.430Z

Inside of the Girder environment that error is triggered if not `os.path.isfile(path)`. Is there some subtle character encoding difference? If you run “girder shell” in the same environment as girder, and then call `os.path.isfile` with the path as explicitly printed in the error message, I assume it will fail in the same way. Once in that environment, you’ll have to figure out how the path that Girder has differs from the path you reach in the terminal.


---

## rbaustin on 2020-02-07T20:37:30.910Z

I entered the Girder shell and ran the command as you described, and at first it tole me the name `os` was not defined. After importing os, I was able to run the command successfully:


`os.path.isfile('/home/ubuntu/studies/rsync-mirror/19-600 Crinetics/19-600 G.1- 01.mrxs')`


Which returned: `True`


---

