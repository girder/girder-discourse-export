# problem installing girder_web_components

## curtislisle on 2021-04-19T20:14:16.123Z

I had an app that used to use an early version of girder\_web\_components. I updated recently, but now encounter build errors in yarn. Am I using out of data components? Here is the yarn error I see. Any suggestions are appreciated.




---


Failed to compile.


./node\_modules/@girder/components/node\_modules/vuetify/src/components/VWindow/VWindow.sass (./node\_modules/css\-loader??ref–9\-oneOf\-3\-1!./node\_modules/postcss\-loader/src??ref–9\-oneOf\-3\-2!./node\_modules/sass\-loader/dist/cjs.js??ref–9\-oneOf\-3\-3!./node\_modules/@girder/components/node\_modules/vuetify/src/components/VWindow/VWindow.sass)  

Module build failed (from ./node\_modules/sass\-loader/dist/cjs.js):



```
@content($material-light)

```

## ^
Invalid CSS after " @content": expected “}”, was "($material\-light); "
in /home/clisle/proj/nci/code/arbor\_nova/client/node\_modules/@girder/components/node\_modules/vuetify/src/styles/tools/\_theme.sass (line 3, column 5\)


here is the dependcies section from package.json:  

“dependencies”: {  

“@girder/components”: “^3\.0\.3”,  

“bootstrap\-vue”: “^2\.21\.2”,  

“core\-js”: “^2\.6\.10”,  

“d3\-dsv”: “^1\.1\.1”,  

“d3\-scale”: “^3\.1\.0”,  

“d3\-scale\-chromatic”: “^1\.5\.0”,  

“file\-save”: “^0\.2\.0”,  

“file\-saver”: “^2\.0\.2”,  

“jquery”: “^3\.6\.0”,  

“material\-design\-icons\-iconfont”: “^4\.0\.5”,  

“node\-sass”: “^4\.12\.0”,  

“openseadragon”: “^2\.4\.2”,  

“postcss\-import”: “^12\.0\.1”,  

“postcss\-url”: “^8\.0\.0”,  

“pug”: “^2\.0\.4”,  

“pug\-plain\-loader”: “^1\.0\.0”,  

“resonantgeo”: “^1\.16\.1”,  

“sass\-loader”: “^7\.3\.1”,  

“stylus”: “^0\.54\.7”,  

“stylus\-loader”: “^3\.0\.2”,  

“vega”: “^5\.20\.2”,  

“vega\-embed”: “^6\.17\.0”,  

“vega\-lite”: “^5\.0\.0”,  

“vue”: “^2\.6\.6”,  

“vue\-router”: “^3\.1\.3”


---

