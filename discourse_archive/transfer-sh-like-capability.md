# transfer.sh-like capability?

## roni on 2018-06-29T18:44:39.388Z

I ran across this really neat little tool for making it easier to share files from the command line: <https://transfer.sh/>. The basic idea is to use `curl` or any other utility to upload a file to a specific URL; the returned response is a transient URL that can be used to download that file, which will last for a default time of two weeks.


Would we want something like that as a plugin for Girder? I can see it being useful to do this sort of thing with [data.kitware.com](http://data.kitware.com) as an easy way to upload a file without having to create a collection or folder, etc.


roni


---

## roni on 2018-07-16T19:37:10.957Z

Any thoughts on this?


roni


---

