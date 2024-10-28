# girder-dicom-viewer

## finetjul on 2019-03-07T17:12:46.580Z

Hi,


On Ubuntu, I installed and configured a girder 3\.0\.0a3 using pip ($pip install girder\=\=3\.0\.0a3\). It works fine.  

Then I installed the dicom viewer 0\.2\.0a1 using pip ($ pip install girder\-dicom\-viewer).  

However, I get those errors when building the web client ($girder build)



```
ERROR in ./~/@girder/dicom-viewer/main.js
Module not found: Error: Can't resolve 'girder/auth' in '/root/Projects/env/lib/python3.6/site-packages/girder/web_client/node_modules/@girder/dicom-viewer'
resolve 'girder/auth' in '/root/Projects/env/lib/python3.6/site-packages/girder/web_client/node_modules/@girder/dicom-viewer'
  Parsed request is a module
  using description file: /root/Projects/env/lib/python3.6/site-packages/girder/web_client/node_modules/@girder/dicom-viewer/package.json (relative path: .)
    Field 'browser' doesn't contain a valid alias configuration
  after using description file: /root/Projects/env/lib/python3.6/site-packages/girder/web_client/node_modules/@girder/dicom-viewer/package.json (relative path: .)
    resolve as module
      /root/Projects/env/lib/python3.6/site-packages/girder/web_client/node_modules/@girder/dicom-viewer/node_modules doesn't exist or is not a directory
      /root/Projects/env/lib/python3.6/site-packages/girder/web_client/node_modules/@girder/node_modules doesn't exist or is not a directory

```

Does it mean the dicom\-viewer is incompatible with girder 3\.0\.0a3 ? What is my other option to install that plugin ?


Thanks,  

Julien.


---

## Jonathan_Beezley on 2019-03-07T17:16:37.783Z

It was probably broken by <https://github.com/girder/girder/pull/2916>. I think `3.0.0a2` should work with this plugin as is. Otherwise we need to update the javascript references from `girder` to `@girder/core`.


---

## David_Manthey on 2019-03-07T17:17:13.000Z

I’m using the Dicom plugin from the master repo with Girder 3\.0\.0a3, and it works. I think we just need to make a release of the current code.


---

## Jonathan_Beezley on 2019-03-07T17:19:29.850Z

Ah right. I’ll see about updating all of the plugins. Any with a client\-side component will likely need a new release.


---

## finetjul on 2019-03-07T17:35:08.609Z

Until plugins are updated, downgrading girder to 3\.0\.0a2 works for me. (I’d rather not use the master repo).


Thanks !  

Julien.


---

## Jonathan_Beezley on 2019-03-07T17:55:06.967Z

girder\-dicom\-viewer 0\.2\.0a2 has now been released with the fix.


---

## finetjul on 2019-03-08T07:49:31.611Z

Thanks a bunch !


---

