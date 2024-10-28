# How do I Enable Candela Visualizations for csv Files?

## nicholsn on 2017-11-09T07:33:34.229Z

I’m new to Girder and ran into an issue installing the Candela plugin.


I installed Girder version 2\.4\.0 and included all plugins during `pip install girder[plugins]` and also using `girder-install web --plugins-all` using Python 2\.7\.12 on Ubuntu 16\.04\.


After getting the girder\-server running, I enabled the Candela plugin, which rebuilt the client libraries, restarted, and appeared to succeed, but after uploading a csv as a data\-item I do not see any addition GUI in the webapp.


I also tried restarting the girder\-server, which reported that the Candela plugin was loaded, however still no visualization for csv data items.


Any idea what steps I missed? Thanks!


---

## jeffbaumes on 2017-11-09T13:20:01.387Z

That is odd. Does the name of the item end with “.csv”? That is what triggers the Candela visualization panel.


Are any errors reported to the web console if you open developer tools?


---

## nicholsn on 2017-11-09T14:35:21.503Z

Ok , got it. I had created a item without the csv prefix and then uploaded a csv file thinking it would be displayed, when what you need to do is to name the actual item with the csv prefilx or upload a csv file to a folder, which automatically creates an item with the csv prefix.


Its working now, thanks!


---

## jeffbaumes on 2017-11-13T13:59:15.752Z

FYI Girder docs have been clarified concerning the item name:


[http://girder.readthedocs.io/en/latest/plugins.html\#candela\-visualization](http://girder.readthedocs.io/en/latest/plugins.html#candela-visualization)


Thanks for the report.


---

