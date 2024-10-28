# girder build not a command

## notarya on 2019-02-22T00:07:58.951Z

Trying to get girder up and running for the first time, I’ve followed the instructions for CentOS and everything seems to work fine until the girder build step, where it shows that there is no build command. Any suggestions?


Commands available:  

`mount † Warning: could not load plugin. See `girder mount --help`.`  

`serve Run the Girder server.`  

`sftpd Run the Girder SFTP service.`  

`shell Run a Girder shell.`


---

## David_Manthey on 2019-02-22T13:14:40.000Z

I think you have Girder 2\.x installed but are following the Girder 3\.x instructions. If you pip installed Girder from pypi, it pulls version 2\.5\.0 unless you specify a preliminary version for version 3 (e.g., 3\.0\.0a2\). The readthedocs information defaults to the latest version. If you pick “version 2\.5” from readthedocs it will match the version you got from pypi.


For Girder 2\.x, the build command is “girder\-install web”. See [https://girder.readthedocs.io/en/v2\.5\.0/installation.html](https://girder.readthedocs.io/en/v2.5.0/installation.html)


* David

---

## notarya on 2019-02-22T19:19:28.389Z

Thanks! I must have missed something in the docs, I had installed the maintenance branch and your suggestion worked!


---

