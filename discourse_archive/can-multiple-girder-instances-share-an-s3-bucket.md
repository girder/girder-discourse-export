# Can multiple Girder instances share an S3 bucket

## curtislisle on 2021-02-18T13:56:46.647Z

I have an application which could have several Girder 3 instances (on separate machines at different sites) all accessing a shared assetstore. If we put the assetstore on an S3 bucket, what problems or contention might we have? Will there be problems sharing a single asset store?


We do want to achieve shared access to files stored through Girder. The application is such that several “sites” would each be contributing to different portions of the common archive. Due to our use case, it is unlikely that multiple Girder instances would ever collide writing the same resource. Most of the time, our use case is that site \#1 will upload files to the assetstore and site \#2 might read the items and change only their metadata records later…


Thanks for any suggestions or warnings about this use case ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=9 ":slight_smile:")


---

## Zach_Mullen on 2021-02-19T12:16:26.929Z

Hi Curt!


Could you clarify what you mean by separate instances that share an assetstore? An assetstore is unique to a single “instance” of Girder, but a single “instance” of Girder can have multiple web servers at different sites, they’d just need to all be pointing to the same Mongo database. If that’s what you mean, it’s totally fine. If you mean three separate mongo databases that all try to share a single assetstore, I’m not sure how that could be feasible. If that’s what you want, I think it would be better to add a layer on top of the assetstore that could do “federated” data lookups in the other instance. But I don’t fully understand the use case here to be able to recommend an approach.


Thanks,


Zach


---

