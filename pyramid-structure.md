# Pyramid Structure

## Saipraneeth99 on 2021-03-19T10:23:24.707Z

Whenever we upload a tiff or svs file or any WSI, where is the pyramid structure created? Using which library? How can we access it?


---

## David_Manthey on 2021-03-19T12:13:28.433Z

The [large\_image library](https://github.com/girder/large_image) is used for reading tiled images. When enabled in Girder, there are specific tile endpoints for getting metadata, individual tiles, regions, thumbnails, etc. These are under the `item/{id}/tiles` endpoints. For instance, `item/{id}/tiles/zxy/{z}/{x}/{y}` will request a specific tile. There are many options, such as returning tiles in either jpeg or png format.


---

## Saipraneeth99 on 2021-03-20T05:24:57.855Z

Yes, Large\_image library and other tilesources are used for reading the images. Is any pyramid structure getting generated? Like is it getting stored at some place physically ?So that when a request like this `item/{id}/tiles/zxy/{z}/{x}/{y}` is called , from where is the tile getting generated ? Can i know the logic for getting a specific tile at some level? From where is that specific tile returned? How the tile is created on WSI?


---

## David_Manthey on 2021-03-22T12:10:40.093Z

The pyramid structure is already present in whatever file is being read. Depending on the file format, different tile sources are used to read it. Some, like tiff, are expected to have multiple images within them stored as a pyramid of tiles . Others, like Nikon (nd2\) are more opaque to large\_image’s code – I think these use a wavelet compression where a region is decoded until the needed resolution is obtained.


Specifically, when a tiff file is created with a pyramid of tiles, the whole image is partitioned into tiles and saved as an image in the tiff file (we use libvips to do this when we convert a file). The image is downsampled to produce a half resolution image (every set of 2x2 tiles is downsampled to form a single lower resolution tile); this reduced resolution image is then appended to the tiff tile. This is repeated for each level in the pyramid. The libvips code has the details.


---

