# Restricting annotations

## RazerEdge64 on 2021-06-24T06:49:46.760Z

This is a tile view for example.  

[![image](https://discourse.girder.org/uploads/default/optimized/1X/e5b5b7ccf5d5d3e1a98a62dd3b81b6f2b734eb00_2_482x500.jpeg)
image522×541 76 KB](https://discourse.girder.org/uploads/default/original/1X/e5b5b7ccf5d5d3e1a98a62dd3b81b6f2b734eb00.jpeg "image")


This is just a tile view for the WSI. It is a WSI but zoomed in to max level and with restricted coordinates of that tile.  

So the issue is, while creating an annotation on this view, If I drag my curser out of this view, the annotation is still created on the big WSI image (ImageView) as big as the curser was dragged. I want to avoid this behavior.  

Is there a way so that I can restrict the annotation to only this view?


---

## David_Manthey on 2021-06-24T12:45:18.880Z

You would have to add some code to accomplish this. The code would depend on your desired behavior – should the mouse “stick” to the edges of your area? Or should an annotation dragged outside the area be discarded? Or should the annotation be modified to crop it to the area after it is drawn? Either way, this would probably involve hooking one of the geojs events (for instance [geo.event.annotation.coordinates](https://opengeoscience.github.io/geojs/apidocs/geo.event.annotation.html#.event:coordinates) or the annotation’s mouse click handler).


– David


---

