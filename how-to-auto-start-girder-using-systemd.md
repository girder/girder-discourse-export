# How to auto-start Girder using systemd?

## abertelsen on 2018-07-27T07:46:46.410Z

Dear all,


This is my first post on the Girder Discourse forum, so I will begin by greeting you all!


I have installed Girder on an Ubuntu 16\.04 machine and was wondering if there is a way to auto\-start \-and auto\-reload\- Girder using systemd. I am able to auto\-start apache2 and mongod using systemd, but I would like to do the same with Girder, to avoid having to reconnect and execute `girder-serve` manually every time.


I have followed Digital Ocean’s [tutorial on systemd](https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-1-practical-examples#auto-starting-services-with-systemd), which has been quite useful. I must admit that I am no expert in server configuration, so I apologise if my question is too basic.


Thanks in advance for your help!


Cheers,  

Álvaro B.


---

## Jonathan_Beezley on 2018-07-27T11:51:36.857Z

Welcome Álvaro,


This is an example systemd unit definition from our [ansible role](https://github.com/girder/girder/blob/master/devops/ansible/roles/girder/templates/daemon/girder.service.j2). You will need to change some values to match your installation.



```
[Unit]
Description=Girder
After=network.target

[Service]
User={{ girder_user }}
Group={{ girder_group }}
Restart=always
ExecStart={{ girder_virtualenv }}/bin/python -m girder

[Install]
WantedBy=multi-user.target
```

---

## abertelsen on 2018-07-27T12:46:55.000Z

Thanks Jonathan! Using your template I was able to make Girder start  

automatically on my machine.


Cheers,  

Álvaro B.


---

