# Freeswitch mod_xml_curl demo

## Dependencies

* Nodejs 6.x.x [link to install](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
<<<<<<< HEAD
  * I used nodejs/express to make a simple webserver to serve the dialplan.xml for freeswitch, just because of it's simplicity,**all autoload_confings are ran from the nodejs directory except for xml_curl.conf.xml.**
* Docker [link to install](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts)
  * To deploy freeswitch, **not** mandatory, but it's easier to deploy using it and some other changes may be needed
  * sudo docker run -d --name freeswitch -p 5060:5060/tcp -p 5060:5060/udp -p 5080:5080/tcp -p 5080:5080/udp -p 8021:8021/tcp -p 7443:7443/tcp -p 60535-60635:60535-60635/udp -v \<PATH_TO_CONF\>:/usr/local/freeswitch/conf bettervoice/freeswitch-container:1.6.16
  * **Important** this docker image places freeswitch's executable in ```/usr/src/freeswitch``` but loads configuration files from ```/usr/local/freeswitch```, also the configuration files are placed in ```../conf/vanilla``` directory instead of the default ```../conf``` directory.
=======
  * I used nodejs/express to make a simple webserver to serve the dialplan.xml for freeswitch, just because of it's simplicity
* Docker
  * To deploy freeswitch, not mandatory, but it's easier to deploy using it and some other changes may be needed
  * sudo docker run -d --name freeswitch -p 5060:5060/tcp -p 5060:5060/udp -p 5080:5080/tcp -p 5080:5080/udp -p 8021:8021/tcp -p 8080:8080/tcp -p 7443:7443/tcp -p 60000-60050:60000-60050/udp -v \<PATH_TO_CONF\>:/usr/local/freeswitch/conf bettervoice/freeswitch-container:1.6.16
>>>>>>> 87968c8305b59a42b82a71387043b08ab0afde8c

## How to

1. Checkout the project :)
2. Change the ip at the ./conf/autoload_modules/xml_curl.conf.xml and ./conf/vars.xml
    * On vars.xml we need to change the ip freeswitch will use as domain, for aws instances I used the instance public ip, the ip (line 26) will be the IP of your domain/freeswitch server.
    * On xml_curl.conf.xml we need to show where is the service that will provide the dialplan xml(if it's on the same address you can leave gateway-url's as ```$${domain}``` if not put the IP address of yournodejs service).
    * If using docker, on acl.conf.xml change the cidr on line 26 to the local address of your container, for example if your local IP is "178.18.0.10" change it to ```<node type="allow" cidr="178.18.0.0/16"/>```(this tells freeswitch to allow requests to/from your docker ip range).  If you are not using docker, delete that line.
3. If using docker, start the freeswitch container using the command above. Remember to change \<PATH_TO_CONF> with the path to the conf folder of this project (/home/ubuntu/xml_curl_test/conf)
4. Start the dialplan webserver.
    * Go to the dialplan folder
    * Run ```node app.js```

Now it should be working, just register one of the default freeswitch users and try to make a call.

Look at ./dialplan/dialplan.xml to see what extensions are available to use.


