# HistomicsTK's server and third party image analysis software

## sfarag on 2020-01-27T16:54:54.047Z

Hi,


Can HistomicsTK’s server works with a third party image analysis software such as orbit:  

Link: <http://www.orbit.bio/>.


If yes, how to make the orbit tool communicate with the HistomicsTK’s server (URL, password, port)


Best regards,


Sherif


---

## David_Manthey on 2020-01-27T17:18:36.000Z

HistomicsTK’s server is Girder. Girder has a variety of ways it can perform authentication – api tokens, oauth, ldap, etc. I haven’t used orbit.bio; I imagine you might need to add a java class to browse images and to read image tiles from Girder’s large\_image plugin.


* David

---

