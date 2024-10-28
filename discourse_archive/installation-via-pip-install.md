# Installation via pip install

## markbuckup on 2020-07-20T20:29:55.509Z

I’m trying to install through Anaconda and am trying the simple "pip install histomicstk \-\-find\-links [https://girder.github.io/large\_image\_wheels."](https://girder.github.io/large_image_wheels.%22) This is my result:


[![Screen Shot 2020-07-20 at 16.20.51](https://discourse.girder.org/uploads/default/optimized/1X/3bbe6b13f1a91c02d0f108447d87fcce48cc94c3_2_690x95.png)Screen Shot 2020\-07\-20 at 16\.20\.511670×232 42 KB](https://discourse.girder.org/uploads/default/original/1X/3bbe6b13f1a91c02d0f108447d87fcce48cc94c3.png "Screen Shot 2020-07-20 at 16.20.51")


Any thoughts?


---

## David_Manthey on 2020-07-20T20:37:45.750Z

What OS are you running? We only have wheels published for linux, so if you are using OSX or Windows, I wouldn’t expect this to work. If you are on a linux of any sort, I would only expect this error if your system can’t reach pypi where histomicstk is published.


* David

---

## markbuckup on 2020-07-20T20:52:16.747Z

That makes sense. I was trying to run on Mac OS. It is my understanding, then, that I install large\_image as a python package and proceed to download HistomicsTK through:


$ git clone <https://github.com/DigitalSlideArchive/HistomicsTK.git>  

$ cd HistomicsTK  

$ pip install \-e .


The issue is that when I clone large\_image and install the dependencies, this error pops up:


[![Screen Shot 2020-07-20 at 16.45.48](https://discourse.girder.org/uploads/default/optimized/1X/2a40dd843e962585678bf432a2ddf34bdbc36e0a_2_690x205.jpeg)Screen Shot 2020\-07\-20 at 16\.45\.481338×398 237 KB](https://discourse.girder.org/uploads/default/original/1X/2a40dd843e962585678bf432a2ddf34bdbc36e0a.jpeg "Screen Shot 2020-07-20 at 16.45.48")


Any idea? I can’t seem to download an updated mapnik either


---

## David_Manthey on 2020-07-21T12:39:55.908Z

Currently, we list `large-image[sources"]` as a dependency in HistomicsTK. This installs all the available tile sources, but each of those requires the appropriate libraries. On linux, we have wheels that satisfy all of these. I know other OSX users have changed `setup.py` so instead of requiring `large-image[sources]`, it just requires `large-image`. You’ll also need to install at whatever tile sources are needed for your image formats. For instance, if you are using svs files, you’ll need to use homebrew to install openslide, then install `large-image-source-openslide`.


I’ll make a change to HistomicsTK’s setup.py file to *not* install tile sources on non\-linux systems, which at least will remove the need to change `setup.py`.


---

## Jon_Sauer on 2022-01-15T20:22:36.071Z

I’d love to use HistomicsTK’s color normalization and related facilities on my Intel MacBook Pro, but of course by Apple’s making clang the default for all C\-related compilers and other things installation via


$ git clone [GitHub \- DigitalSlideArchive/HistomicsTK: A Python toolkit for pathology image analysis algorithms.](https://github.com/DigitalSlideArchive/HistomicsTK.git)  

$ cd HistomicsTK  

$ python3 \-m pip install \-e .


is full of errors. Thanks for changing HistomicsTK’s setup.py file, but it still looks like a non\-trivial time sink to get HistomicsTK installed on a Mac. Perhaps you could point me to a friendly user that has succeeded?


If not, I’ll try to use use the techniques in “Ensemble Prostate Tumor Classification in H\&E Whole Slide Imaging via Stain Normalization and Cell Density Estimation” by Weingnat et. al. which seem pretty straightforward (before actually trying them).


Cheers,  

Jon Sauer


---

