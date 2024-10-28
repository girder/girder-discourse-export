# Overlay 2 images

## rogergenbot on 2021-10-04T18:05:47.867Z

Hi all, I was wondering if is there any way to display two images at the same time in DSA. I’m working on registering whole slide images and it’d be interesting to have some kind of tool to blend two images with opacity control?  

Thanks!


---

## David_Manthey on 2021-10-04T18:14:53.794Z

There isn’t any built in way to do this. The underlying rendering libraries support it fairly easily – we did a proof\-of\-concept where we could even show an affine transform between multiple images that could change depending on the location viewed.


Some other projects have added code that might do this (for instance, I think ABCS Fredericks did so), but it isn’t integrated into the master code. We’d love to have those options contributed upstream, but haven’t had availability to do it ourselves.


David


---

## rogergenbot on 2021-10-04T18:20:34.000Z

Thanks David! Do you know if Fredericks project is open source as well?


---

## David_Manthey on 2021-10-04T18:23:24.059Z

They have a Github organization with numerous repositories. One that might be relevant is [GitHub \- abcsFrederick/overlays: Girder plugin for creating and displaying image overlays.](https://github.com/abcsFrederick/overlays), but they have also forked some of the upstream repositories. I haven’t looked at them enough to know easy it would be to generalize their work.


---

## rogergenbot on 2021-10-04T18:24:02.000Z

Thank you very much!


---

