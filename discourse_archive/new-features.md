# New features

## Nikoleta on 2019-12-08T21:35:16.979Z

Hello,


we would like to use HistomicsTK as a visualization tool for our university project concerning cancer detection on tissue. Although it has many functionalities we need, we still lack some modifications, mainly:


* Add a new layer over existing slide with explainability image (e.g. saliency map)
* editing existing annotations
* add new sliders (as each annotations layer need to have its own opacity)


Is any of these features something you are planning to add in the near future?


And if not, would it be possible to provide us with some detailed architecture of your code or point us to a script where we could make these changes so we could contribute?


Thank you in advance,


Nikoleta


---

## David_Manthey on 2019-12-09T19:44:16.129Z

We don’t have immediate plans for adding these features – we’ve speculated on adding them eventually, but have no time line.


I’ve seen a demo that others have written which adds saliency map overlays. They did it by adding an additional panel to the interface (modifying web\_client/panels), but I don’t think that group has shared any code.


There is code for editing individual annotations in the rendering library we use (geojs), but it hasn’t been exposed in the UI. This would probably involve moving the element to be edited to a geojs annotation layer (much like when the annotation element is first created) and switching the renderer to edit mode.


If you start to do this, we’d love to get contributions back to the code base.


* David

---

