# HistomicsTK on Windows

## Abdul_Rahim on 2021-08-28T05:39:35.377Z

Noob here . I’ve been looking for a python package to work with H\&E stained histopathological images. Came upon this. I wanted to install this on Anaconda Environment in Windows 7 .Followed this link [Guide to Windows Installation · Issue \#855 · DigitalSlideArchive/HistomicsTK · GitHub](https://github.com/DigitalSlideArchive/HistomicsTK/issues/855) line by line to install on Windows. But by the time I try to use \`python \-m pip install \-e .’


After running setup.py ,The following error occurs.  

![Error](https://discourse.girder.org/uploads/default/original/1X/d8c418e5547327a449873b146b32c75b01b20a8f.jpeg)


Is there any other way to install histomicstk on Windows . I want to use it on an Anaconda environment for my projects.


---

## David_Manthey on 2021-09-06T13:08:16.366Z

We don’t test on Windows, and Windows 7 is now considered too old for support. You can run HistomicsTK on Windows 10 in Docker Desktop with WSL enabled, or use Vagrant to get a virtual box that can run things. Otherwise, you *probably* can install a bunch of libraries and get it working, but we don’t have direct experience doing so.


– David


---

