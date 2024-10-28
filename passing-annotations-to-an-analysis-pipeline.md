# Passing annotations to an analysis pipeline

## ewoud on 2020-03-09T20:08:41.105Z

Hi,


I would like to pass annotations made on an image to an analysis pipeline, however, when I make some annotations in the interface and go to the nuclei classification demo, which ask for an annotations file, I can not select the annotations I have just made on the image. Is there a way to do so?


Thanks in advance!


---

## David_Manthey on 2020-03-09T20:14:41.885Z

There isn’t currently a way to do that directly. We’ve had another request to do this, but we haven’t gotten around to coding it.


There is a work\-around that doesn’t involve adding code: you can save one or all annotations to a file and then pass that file (either by uploading it back to Girder or specifying it from a path that can be read by the job).


Ideally, we’d add an “annotation” type to the slicer\_cli\_web module to allow you to select an annotation and pass it through. As a first step to this, we would want a bit of code for that would run in girder\_worker that would fetch the annotation from girder (via girder\_client). The UI would then be able to pass an annotation ID to that code.


* David

---

