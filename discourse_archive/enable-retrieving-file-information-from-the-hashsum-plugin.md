# Enable retrieving file information from the Hashsum plugin

## Michael_Grauer on 2017-11-17T01:23:22.372Z

Eric:


For determining if a collection is underneath a parent, I think it’d be simplest just to check the resource path of the item to see if it’s a child of the desired folder (rather than doing server\-side recursive id queries?).


Mike:


I think I’m confused by what you mean as a collection here. In Girder, collections are top level resources that don’t have parents.


You can check the resource path of the item to see if it is a child of the desired folder, or if the desired folder is a top level folder (e.g. a collection) you can check by looking at the item’s baseParentId.


Eric:


Looking at \#[2458](https://github.com/girder/girder/pull/2458), might it be relatively close? (Also, would it be hard to / negatively affect performance if the item resource path was thrown into the response as well to save the extra queries, if that’s not too much scope creep?)


Mike:


This should be close as it currently is, I’ve posted the additional ask on that PR. Without this addition it is probably just a couple hours or so to add a test and get it across the finish line.


---

## EricCousineau-TRI on 2017-11-17T23:47:07.028Z


> I think I’m confused by what you mean as a collection here. In Girder, collections are top level resources that don’t have parents.


Hup, sorry! I had misspoke (\-typed?), I had meant “For determining if a**n item** is underneath a parent, \[…]”.



> This should be close as it currently is, I’ve posted the additional ask on that PR. Without this addition it is probably just a couple hours or so to add a test and get it across the finish line.


Awesome!


---

## Michael_Grauer on 2017-11-17T23:51:12.796Z

[@EricCousineau\-TRI](/u/ericcousineau-tri)


I’m leaning towards not adding the item resource path because of the additional lookups per file, and this would allow us to get it merged in sooner.


Would that work for you, then you could check the resource path of the item with one additional API call on the client side? We could look to alter this in the future if becomes a burden for you, and we can come to an agreement as to how to implement it on the server side.


---

## EricCousineau-TRI on 2017-11-17T23:51:50.471Z

Yup, that sounds good!


---

