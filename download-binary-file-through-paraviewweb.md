# download binary file through ParaviewWeb

## Charles on 2019-03-19T14:54:39.739Z

I am using the [downloadFile](https://github.com/Kitware/paraviewweb/blob/master/src/IO/Girder/CoreEndpoints/api.md) method from [paraviewweb](https://github.com/Kitware/paraviewweb), and I have some troubles with binary files. This method return a Promise, which contains the content of the file in its “data” field. However for binary files, I don’t have the good value for this filed, it differ from the original file.


Using wireshark, I can say that the information sent by the server is the good one. It seems there is a problem in the way paraviewweb decode it.


Is there any existing project using the downloadFile method for binary files ?


---

## Zach_Mullen on 2019-03-19T15:09:59.234Z

Hi Charles,


Could you open an issue for this in paraviewweb?


---

## Charles on 2019-03-20T09:09:27.276Z

Hello Zach,


I have created the issue [\#492](https://github.com/Kitware/paraviewweb/issues/492).  

If you need further information or testing, feel free to contact me.


---

