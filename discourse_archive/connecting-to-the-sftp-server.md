# Connecting to the sftp server

## Edern_Haumont on 2019-03-14T17:19:45.530Z

After fighting a bit with paramiko’s ssh key reading system, I could finally run the SFTP service a.k.a. “girder sftpd”.


However I am stuck when trying to connect with a client.


* When using curl, I get a timeout (15s) and a server side error “Error reading SSH protocol banner”
* When using Thunar (a file explorer with ftp support) I instantly get the same error on the server
* When using Putty, the connection seems to be established but after entering my credentials or “anonymous” I get that error on the client: “server refused to allocate pty”. This time no error on the server side except a deprecation notice. Nothing in the logs.


I could not find in the docs if I missed something. Do you have some pointers ?


Thanks for your help.


Edern


---

## Zach_Mullen on 2019-03-14T17:56:28.262Z

What connection info are you using on the clients? First I’d make sure the protocol and ports match. (Protocol should be “sftp:”)


---

## Jonathan_Beezley on 2019-03-14T18:02:04.044Z


> * When using curl, I get a timeout (15s) and a server side error “Error reading SSH protocol banner”


That would be expected because curl doesn’t contain an sftp client.



> * When using Thunar (a file explorer with ftp support) I instantly get the same error on the server


Sounds like this is connecting as a normal ftp client. As Zach mentioned, try forcing it to connect through an ssh tunnel.



> * When using Putty, the connection seems to be established but after entering my credentials or “anonymous” I get that error on the client: “server refused to allocate pty”. This time no error on the server side except a deprecation notice. Nothing in the logs.


This looks like putty trying to open a shell through a normal ssh connection. It looks like putty does support sftp, but I’m not sure where to look for it.


The easiest way to check if everything is correct is to try connecting through the commandline sftp client on Mac or Linux… That would be:



```
sftp sftp://username@hostname:port

```

If you can connect with that, then it’s just a configuration problem on your client.


---

## David_Manthey on 2019-03-14T18:21:07.265Z




![](https://discourse.girder.org/user_avatar/discourse.girder.org/jonathan_beezley/48/16_2.png) Jonathan\_Beezley:

> This looks like putty trying to open a shell through a normal ssh connection. It looks like putty does support sftp, but I’m not sure where to look for it.



Putty has a sftp client (called psftp) that takes mostly the same parameters as linux’s sftp command. You should be able to use `psftp sftp://username@hostname:port`.


---

## Edern_Haumont on 2019-03-15T08:11:00.864Z

Thanks all for your answers,  

Indeed in the first two cases I wrongly assumed the clients were connecting in sftp while they were actually trying to connect through normal ftp.  

For putty however, given that it was trying to connect via ssh, I am not sure what the problem exactly was.


Anyways directly using psftp in the CLI worked perfectly !


---

