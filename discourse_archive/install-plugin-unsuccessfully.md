# Install plugin unsuccessfully

## sunaifei on 2022-01-27T05:54:25.993Z

I don’t see the gear icon in ‘Admin console’ \- ‘Plugins’ \- ‘HistomicsUI’  

I install HistomicsUI by following steps：


1. pip install HistomicsUI
2. girder build  

When I execute girder build:  

[![8242d915d9dd49a08ad03c1441d8de9](https://discourse.girder.org/uploads/default/original/1X/5bebab1d6419d6d0828c309be571a22f081d13ee.png)
8242d915d9dd49a08ad03c1441d8de91134×633 31\.8 KB](https://discourse.girder.org/uploads/default/original/1X/5bebab1d6419d6d0828c309be571a22f081d13ee.png "8242d915d9dd49a08ad03c1441d8de9")
  

I found the copy\-webpack\-plugin folder in node\_modules, but the package.json in the folder shows that the version of copy\-webpack\-plugin is 4\.6\.0, which is inconsistent with the version requirement of girder’s dependencies (4\.5\.2\) . I don’t know if this is the reason why HistomicsUI doesn’t work in girder\_client.

---

