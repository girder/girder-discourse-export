# Annotations inside a Region

## RazerEdge64 on 2021-05-21T07:52:14.484Z

Hi devs,  

Let’s say I want to get all the annotations inside a region for example, the point ,ellipse and polyline in the below region (rectangle).


[![image](https://discourse.girder.org/uploads/default/optimized/1X/d4063e07643a623d254d4359611586be41ed5275_2_690x438.jpeg)
image907×576 334 KB](https://discourse.girder.org/uploads/default/original/1X/d4063e07643a623d254d4359611586be41ed5275.jpeg "image")


Assuming all the elements are inside one new Annotation.  

This is what this annotation would look like  

rectangle, // This would be the roi  

polyline,  

ellipse,  

point


The problem is,


1. How would we identify that which is the ROI if there are multiple ROIs for one WSI ?
2. After identifying the ROI, how do we know what annotations are present inside that ROI ?
3. If I assume that when I create a new annotation, one of the element is the ROI and all of the other elements are inside the ROI. What if there are multiple rectangles?  

( we could iterate all the elements for the type rectangle and find the one with max height and width but, laborious )




---


TL;DR  

What is the best way to handle region based annotations ?  

Any suggestions would be greatly appreciated!


---

## David_Manthey on 2021-05-25T17:07:46.589Z

The semantic meaning of the annotations isn’t stored in any manner – you have to either name them or have a consistency for your project. In other projects, there are frequently annotations explicitly called “ROI” often with a suffix (e.g., “ROI 1”). Other annotation elements are usually stored in another annotation.


Often the additional annotations won’t strictly end at the ROI. It can be conceptually easier for the pathologist to mark a structure across the ROI boundary.


If you are accessing the annotations via the REST endpoint, the endpoint supports spatial limits. That is, the endpoint `GET /annotation/{id}` can take left, right, top, and bottom parameters. Only annotation elements whose bounding boxes cross the specified region are returned. For annotations with numerous elements, you can also limit the number of elements returned. Given an ROI, passing that to the endpoint could then return just the elements of the annotation that are at least partially in the ROI.


Naturally, if your project has specific needs or would benefit from a specific workflow, these details could be hidden with some custom API added as a Girder plugin.


– David


---

