# Accelerator
Free Online Conference and Collaboration Tool with build in Node SFU

![previmg](/public/images/acc.png)

### Available functions ###

- [x] Online Conference with "*unlimited*" participants (Audio / Video)
- [x] Screenshare
- [x] Presentations
- [x] Whiteboard ([Repo](https://github.com/cracker0dks/whiteboard))
- [x] Youtube viewer
- [x] 3D Object viewer
- [x] User interactions with draggable items (like Textboxes, Drawings...)
- [x] Fileshare
- [x] Text Chat
- [x] WebRTC SFU in Node (using [simple-peer](https://github.com/feross/simple-peer))
- [x] Configurable with own STUN / TURN Server (Setup below)
- [x] Mutch more...

- [ ] Etherpad (Must be hosted on its own)

### Installation without Docker ###
1. install nodeJs
2. run: npm install
3. run: node server.js
4. surf to: https://127.0.0.1

### Docker Installation ###
1. build . -t acc
2. run: docker run -d --net=host acc
3. surf to https://yourIp

To have all persistent datas (config, rooms, presentations...) outside of docker, you can run it like this:

```
docker run -d --name acc --net=host -v /home/acc/config:/opt/app/config -v /home/acc/db:/opt/app/db  -v /home/acc/3dObjs:/opt/app/public/3dObjs -v /home/acc/praesis:/opt/app/public/praesis -v /home/acc/profilePics:/opt/:/opt/app/public/profilePics -v /home/acc/singlefiles:/opt/app/public/singlefiles acc
```
### Configuration ###
On the first start a new folder "/config" will be generated. Take a look at "/config/config.json" for all parameters. Change them if you like, and restart the server. More to come...

### ToDos ###

- [ ] Better error feedback
- [ ] More, better docs
- [ ] Better Etherpad configuration
- [ ] Implement Audio as MCU and Video as SFU
- [ ] Recording of Audio/Video (Prototyp working)
- [ ] Convert WebRTC Streams to RTMP so we can stream to youtube/twitch live (Prototyp working)

### GoodToKnow ###

* Audio/Video is not Peer2Peer so it will use some server CPU
* Firefox sometimes has some issues with the WebRTC audio/video, use chrome to be save
* If you are running without docker, conversion to PDF presentaions (From Powerpoint and other Docs) will not work without installing "unoconv" on your own 
* Setup a TURN Server if your clients are behind Firewalls and NATs (See config, setup below)
* This was/is a student project and was build in 1 Semester so don't expect enterprise code :)

### TURN Setup/Configuration ###
1. Setup your TURN Container on an extra Server: [HowTo](https://github.com/cracker0dks/turn-server-docker-image/blob/master/README.md)
2. Make a new "iceServers" entry in "/config/config.json"
```
{
	"url": "turn:IP_TO_TURN:443",
	"turnServerCredential": "authSecret",
	"username": "webrtcuser"
}
```
- "username" can be anything you like.
- "turnServerCredential" must be the "authSecret" form the TURN Server installation.

Restart the server.

### Behind a nginx reverse Proxy ###
```
location /acc/ {
	resolver 127.0.0.1 valid=30s;
	proxy_set_header HOST $host;
	proxy_http_version 1.1;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection upgrade;
	proxy_pass https://serverIp:port/;
}
```
Because we are running with --net=host, we ca set "serverIp" to the ip of the host.

-------------------------

Based on a Semester Project at: [Reutlingen University](https://www.reutlingen-university.de)

### Authors ###
* Simone Liegl (simone.liegl@gmail.com) | Frontend / Design / UX
* Raphael Fritsch (raphael.fritsch@gmx.de) | Backend / WebRTC
* Sebastian Hirth | Frontend / Backend / Logo

license: GPLv3.0
