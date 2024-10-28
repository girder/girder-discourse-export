# Shutdown procedure

## Edern_Haumont on 2019-04-03T12:44:48.791Z

Hello everyone,


I could not find the information in the code or the documentation: is there an event or an exception that is sent by girder when it shuts down ?


This is my use case: the `load` function of my plugin starts a thread which runs a sftp server besides its other functionalities. The thread runs a lot like the “sftpd” cli. However I do not know what would trigger the call to end sftp serving (in a clean way) ie `server.shutdown()` and/or `server.server_close()`.


Did I miss some documentation ? In general, has girder a way to tell the plugins it is going to exit, for instance the opposite of `load` functions, or a special event/exception ?


Thanks a lot.


---

## Zach_Mullen on 2019-04-04T12:18:40.337Z

Girder doesn’t provide a hook for this, but you should be able to hook directly into cherrypy’s bus hooks to do this.


I’m not condoning the idea in general, just saying what is possible ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=8 ":slight_smile:")


---

