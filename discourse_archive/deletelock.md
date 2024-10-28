# deleteLock

## finetjul on 2019-09-29T19:27:24.129Z

Hi,


I have multiple deleteLock files in my filesystem assetstore. Is it normal ? Can I delete those files and more importantly, can I delete the file without the deleteLock suffix ?  

Should I create a cleanup script?


Thanks,  

Julien.


---

## David_Manthey on 2019-09-30T13:18:30.000Z

The Girder filesystem assetstore dedups identical files, so if you upload two files with exactly the same contents, the contents are only saved once. The deleteLock files are created to avoid a problem that could occur when deleting a file while uploading an identical file. It looks like the library we use for lock files stopped deleting lockfiles as of a version published in the middle of last year. The deleteLock files do no harm (other then taking up directory space), won’t prevent deleting the file without the suffix (unless you are uploading an identical file at the same time), and can be removed if you know the companion file isn’t being deleted.


---

## finetjul on 2019-09-30T14:20:29.379Z

Thanks for your answer.


I need to investigate a bit more concerning my filesystem assetstore…


Thanks,  

Julien.


---

