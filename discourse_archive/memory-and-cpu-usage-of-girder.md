# Memory and CPU usage of Girder

## finetjul on 2018-07-04T10:13:46.854Z

Hi,


I would like to know how much memory Girder and all its dependencies (e.g. MangoDB) is typically consuming at run time.  

Do you have some idea (even rough) ? Is the consumption varying over the number of connected users ? The size of the database ?


Do you have some minimum requirement for the number of CPU cores required ?


Thanks for your information,  

Julien.


---

## David_Manthey on 2018-07-05T12:15:53.768Z

This will depend heavily on the plugins you have enabled in Girder and how many data records you have. MongoDB will typically be the biggest memory consumer, and following its recommendations will be sufficient for Girder.


As a few points of reference, an active system that has \~500 users and \~300,000 items runs fine in a 4Gb system. Within that Girder reports using resident memory of \~128Mb, Mongo reports about the same, and the bulk of the memory is used for OS level file system caching. On a second system that have only a few users and uses plugins for extensive image analysis, Girder is using \~4 Gb of resident memory (mostly caching data from the large\_image plugin) and Mongo is using \~1Gb of resident memory.


Girder will run on a single core system. The system mentioned above with \~500 users is only dual\-core. Depending on plugins, you can get better performance with more cores.


* David

---

## finetjul on 2018-07-05T12:26:58.206Z

Thanks David for your answer. That was helpful.


---

