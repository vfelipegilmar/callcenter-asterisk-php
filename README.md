== Callcenter ==

Server process is started:

$> php server.php

It will listen to Asterisk AMI on tcp://127.0.0.1:5038
and create a Websocket server on tcp://127.0.0.1:8080/callcenter

The webpage dashboard is a static HTML/JS page started:

$> google-chrome public/index.html

The webpage will connect to the websocket server and on connection
get the current state of the the backend server process.

Changes from default configuration files in Asterisk:

* sip.conf

```
tcpenable=yes
transport=tcp,udp
autocreatepeer=yes

[caller]
type=friend
host=dynamic
port=5060
nat=yes
sipreinvite=no
transport=tcp,udp
secret=password
callevents=yes
directmedia=no
insecure=invite,port
```

* manager.d/admin.conf

```
[admin]
secret = password
read = call,agent,user,security
write = call,command,agent,user,originate
```

* queues.conf

```
[q-callcenter]
strategy=random
joinempty=yes
leavewhenempty=no
ringinuse=no

setinterfacevar=yes
setqueueentryvar=yes
setqueuevar=yes

timeout = 60
retry = 10
wrapuptime=30

autopause=yes

reportholdtime=yes
announce-position=yes
announce-holdtime=yes
queue-callswaiting = queue-callswaiting
queue-thankyou = queue-thankyou
queue-youarenext = queue-youarenext
periodic-announce = queue-periodic-announce
periodic-announce-frequency=30
announce-frequency=45
```

* extensions.ael

Empty

* extension.lua

Empty
