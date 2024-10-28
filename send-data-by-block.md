# Send data by block

## Edern_Haumont on 2018-05-11T12:29:43.082Z

Good afternoon,


I am currently modifying a web application that loads a volume from a server(say a .vti file) and renders it. To make it more usable on slow internet networks, I created a girder plugin that loads the volume data into an array and split this data array into blocks along one direction. The client application can then request the blocks one by one with corresponding requests, dynamically recompose the volume and render it as data arrays are received.


Now it is starting to work but I am wondering if I am not reinventing the wheel. Is there already a simple way to send data by block/chunk with girder ?


I hope I have been clear enough with my goals.


---

## David_Manthey on 2018-05-11T12:37:50.296Z

Girder supports standard HTTP range requests, so you can ask for a byte range from the original file. This would require the client to either be able to handle partial data or to know enough about the file structure to request byte ranges that correspond to the data that is desired.


There are some other Girder plugins that send partial data, but I don’t think the ones I know about fit your needs. The Candela plugin can subset csv files or json list files. The database\_assetstore plugin can handle database queries with limits and offsets.


---

## Edern_Haumont on 2018-05-11T12:49:40.737Z

Thank you David.  

Yes the client should be able to request for extents and should not know about byte ranges.  

For now, the server only stores the full volume file on disk and split it when loading it in memory. So I guess I will continue with my plugin.


---

