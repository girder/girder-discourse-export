# Muktichannel visualization

## rogergenbot on 2021-08-28T09:01:53.957Z

Hi,  

I was wondering if DSA has/will have support for multichannel data e.g. immunofluorescence ?  

Thanks!


---

## David_Manthey on 2021-09-20T15:58:21.467Z

Internally, we support reading all of these. When you open a tile source, you can just reference a specific channel by adding `frame=<0-based number>`. The UI in Girder on the item page shows the various channels. The UI in HistomicsUI does not show this – there is an issue to make this UI better (see [Better frame selection · Issue \#498 · girder/large\_image · GitHub](https://github.com/girder/large_image/issues/498)), which, depending on funding, will be done sometime late this year or next year.


---

