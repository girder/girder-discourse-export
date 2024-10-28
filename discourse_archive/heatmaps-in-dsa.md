# heatmaps in DSA

## rogergenbot on 2021-06-14T08:20:21.753Z

Hello all, so far I have been using DSA to gather annotations and to displaying my predictions (as polygons). I would like however display my predictions as heatmaps, pretty much as sated in this old gitter msg :  

[![Screenshot 2021-06-14 at 10.19.08](https://discourse.girder.org/uploads/default/optimized/1X/08ee6f28a8e9132add5cc8e2d642b5f894cfaef6_2_690x170.png)
Screenshot 2021\-06\-14 at 10\.19\.081550×382 71\.3 KB](https://discourse.girder.org/uploads/default/original/1X/08ee6f28a8e9132add5cc8e2d642b5f894cfaef6.png "Screenshot 2021-06-14 at 10.19.08")  

Thanks for any help!


---

## David_Manthey on 2021-06-14T12:49:33.131Z

Heatmaps can be upload like any other annotation. There are two formats: see [large\_image/annotations.md at master · girder/large\_image · GitHub](https://github.com/girder/large_image/blob/master/girder_annotation/docs/annotations.md#heatmap) and the section below it.


* David

---

## rogergenbot on 2021-06-14T12:53:04.523Z

Thanks David! So if I have a heatmap for the whole slide, should I list all the pixels? Wouldn’t that be a lot?


---

## David_Manthey on 2021-06-14T13:04:58.742Z

Although it is somewhat slow to load, it should work – I’m not sure I’ve seen it used with more than \~10,000,000 points though. There is a grid form which is more compact if you are doing that ([large\_image/annotations.md at master · girder/large\_image · GitHub](https://github.com/girder/large_image/blob/master/girder_annotation/docs/annotations.md#grid-data)). In other projects, users have downsampled their heatmaps by some factor, storing only every second or forth value.


---

## rogergenbot on 2021-06-14T13:05:57.356Z

That makes sense! Thanks a lot David


---

