# SFTP not working on Ubuntu16.04

## lucie.macron on 2019-04-11T16:13:47.895Z

Hi all,


I can start a sftp server using girder sftpd. However, I cannot connect to it using FileZilla or psftp.  

We tested it in ArchLinux and Mac and everything worked.


Here is the output log of FileZilla on Ubuntu



```
|Status:|Connecting to localhost:8022...|
|Status:|Connected to localhost|
|Status:|Retrieving directory listing...|
|Status:|Directory listing of "/" successful|
|Status:|Retrieving directory listing of "/(unknown date) b'collection'"...|
|Command:|cd "/(unknown date) b'collection'"|
|Error:|Directory /(unknown date) b'collection': received failure with description 'Failure'|
|Error:|Failed to retrieve directory listing|
|Status:|Retrieving directory listing of "/(unknown date) b'user'"...|
|Command:|cd "/(unknown date) b'user'"|
|Error:|Directory /(unknown date) b'user': received failure with description 'Failure'|
|Error:|Failed to retrieve directory listing|

```

Thanks for your help,


---

## lucie.macron on 2019-04-12T11:49:56.687Z

Actually, it was not working with an old version of FileZilla (v\-3\.21\).  

An upgrade to the last version (v\-3\.41\) fixes this issue.


---

