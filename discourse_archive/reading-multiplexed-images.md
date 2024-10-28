# Reading multiplexed images

## sfarag on 2020-04-16T19:07:20.142Z

Is HistomicsTK / large\_image module capable of reading and processing Immunofluorescence images. I tried but no success so far. Please advise!


Best regards.


Sherif


---

## David_Manthey on 2020-04-16T19:58:21.675Z

The large\_image module certainly does. If you are using the Girder item view, you’ll see a control to see different “frames” of images – frames could be different channels of fluorescence, different z\-slices, time slices, stage positions, etc.


The HistomicsUI interface doesn’t expose this (but it easily could). The HistomicsTK toolkit can access any of the image frames by specifying a parameter when asking for an image or tile from an image.


* David

---

## sfarag on 2020-04-17T05:01:56.846Z

Hi David,


Thanks a lot from the prompt reply. I am afraid, I wasn’t able to open any IF images through either histomicsTK module, Large\_image module or even through DSA girder view.


The image that I have: it consists of 3 .svs images (one for each stain) and an .afi file. Usually one should upload all of them together and click on the .afi file to show the final image with all the different staining. So, I would be truly grateful, if you could either guide me to a histomicsTK tutorial with immuno fluorescence images or show me how to do it.


* I also tried to search for Large\_image module documentation / tutoriall, but I am afraid, I couldn’t find any. Moreover, Girder view doesn’t display any “control” option to see different frames of images. Are you running a different version than the one at the github repo ? <https://github.com/DigitalSlideArchive/digital_slide_archive/tree/master/ansible>


Best,


Sherif


---

## David_Manthey on 2020-04-17T12:26:08.211Z

large\_image doesn’t handle Aperio Fused Image (afi) files (it could be extended to do so), so you would just see your three svs files as separate images (not as one image with three frames). We directly work with Nikon nd2 files and many ometiff files.


---

## sfarag on 2020-04-21T14:36:05.469Z

Thanks a lot David, that make sense!


Best,


Sherif


---

