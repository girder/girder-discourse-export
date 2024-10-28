# Unexpected Error

## TerezaSadilova on 2019-07-11T18:58:44.437Z

Hello,  

today, after installation girder\-dicom\-wiever I wanted to start the web aplication as usual but I got this error:


Detected girder.local.cfg, this location is no longer supported.  

For supported locations, see [https://girder.readthedocs.io/en/stable/configuration.html\#configuration](https://girder.readthedocs.io/en/stable/configuration.html#configuration)  

Started asynchronous event manager thread.  

CherryPy Checker:  

‘/home/ubuntu/girder\_env/share/girder/static’ (root \+ dir) is not an existing filesystem path.  

section: \[/]  

root: None  

dir: ‘/home/ubuntu/girder\_env/share/girder/static’


I was not able to fix it. I would be very grateful for advice.  

Thank you  

Best regards  

Tereza Sadilová


---

## danlamanna on 2019-07-11T20:56:56.748Z

It looks like you’ve upgraded from Girder 2 to Girder 3 (which isn’t released yet). This may have unintentionally occurred by installing `girder-dicom-viewer`.


If you’re going to continue with Girder 3, it no longer supports storing the configuration file inside the python package. It needs to be moved to one of the supported locations mentioned in this paragraph of the documentation: <https://girder.readthedocs.io/en/latest/configuration.html>.


---

