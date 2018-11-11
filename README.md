# docker-iobroker
Docker image for ioBroker (http://iobroker.net) based on debian:latest (http://hub.docker.com/_/debian/)

Fork of https://github.com/buanet/docker-iobroker to run on Ubuntu. Added a script to enable multihost support

## Installation & Usage

```
# Install Docker Comunity Edition
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
# Install Local Persist Login
curl -fsSL https://raw.githubusercontent.com/CWSpear/local-persist/master/scripts/install.sh | sudo bash
```
### Sample docker-compose.yml
```
version: '2'

services:
 iobroker:
    image: castorio/iobroker
    restart: always
    privileged: true
    stdin_open: true
    tty: true
    ports:
      - "1880:1880"  #node-red
      - "1883:1883"  #mqtt
      - "8081:8081"  #iobroker admin
      - "8282:8282"  #flot
      - "8088:8088"  #terminal
      - "8284:8284"  #socketIO
      - "8123:8123"  #homekit
      - "50005:50005" #Multihost
      - "9000:9000"  #Multihost
      - "9001:9001" #Multihost
      
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - iobroker:/opt/iobroker
      - /dev/:/dev/
    hostname: iobroker-master
    container_name: iobroker
    

volumes:
  iobroker:
    driver: local-persist
    driver_opts:
       mountpoint: /path/to/storage/iobroker

```

For me it just works with local-persist Adapter plugin
To enable Multihost support just start the included Script. Settings will be persistant on mounted Volume
```
/opt/maintenance_scripts/enable_multihost.sh YOURSECRET
```

## Changelog

### v1.2.1.1 (2018-11-10)
* added multihost script

### v1.2.1beta (2018-09-12)
* added support for firetv-adapter

### v1.2.0 (2018-08-21)
* after testing making 1.1.3beta to latest stable release 

### v1.1.3beta (2018-08-21)
* added ffmpeg-package for yahka to support webcams

### v1.1.2beta (2018-04-04)
* added ENV for timezone issue

### v1.1.1beta (2018-03-29)
* added wget package
* updated readme.md

### v1.1.0 (2017-12-10)
* changed startup call to fix restart issue
* fixed avahi startup issue
* fixed hostname issue
* added z-wave support
* added logging to /opt/scripts/docker_iobroker_log.txt

### v1.0.1beta (2017-08-25)
* fixed locales issue

### v1.0.0 (2017-08-22)
* moved and renamed iobroker startup script
* disabled iobroker deamon to (hopefully) fix restart issue
* added some maintenance scripts

### v0.2.1 (2017-08-16)
* added libfontconfig package (for iobroker.phantomjs)
* added gnupg2 package as prerequisite for installing node version 6

### v0.2.0 (2017-06-04)
* fixed startup issue in startup.sh
* changed node version from 4 to 6

### v0.1.2 (2017-03-14)
* added libpcap-dev package (for iobroker.amazon-dash)

### v0.1.1 (2017-03-10)
* added git package

### v0.1.0 (2017-03-08)
* moved avahi-start.sh to seperate directory
* fixed timezone issue (sets now timezone to Europe/Berlin)

### v0.0.2 (2017-03-06)
* added support for avahi-daemon (installation and autostart)

### v0.0.1 (2017-01-31)
* project started / initial release

## License

MIT License

Copyright (c) 2017 Andre Germann

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Credits

Inspired by https://github.com/MehrCurry/docker-iobroker

Fork of https://github.com/buanet/docker-iobroker 
