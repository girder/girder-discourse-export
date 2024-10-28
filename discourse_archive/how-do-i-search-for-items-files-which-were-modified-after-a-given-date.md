# How do I search for items/files which were modified after a given date

## Steven_Fines on 2018-01-24T19:30:49.873Z

I currently feed data from multiple systems into Girder automatically\- I need to be able to search for files inserted / modified after a specified date so that I can feed the data out to another system(s).


Looking through the API docs I don’t see a public method for performing this task. I was thinking that I would have to do something with metadata but it doesn’t seem like I can do non\-equal searches.


Any ideas?


---

## Zach_Mullen on 2018-01-29T15:45:49.157Z

Hi Steven,


There is currently not any core functionality that will do this out of the box, but it would be pretty straightforward and easy to implement a new endpoint in a plugin that would provide this capability. That plugin could just add a database index for the search you need to perform, and a REST endpoint to do the lookup.


---

