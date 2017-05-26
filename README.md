# Freeswitch mod_xml_curl demo

## Dependencies

* Nodejs 6.x.x [link to install](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
  * I used nodejs/express to make a simple webserver to serve the dialplan.xml for freeswitch, just because of it's simplicity, all autoload_confings are ran from the nodejs directory except for xml_curl.conf.xml.
* Docker [link to install] 
(https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts)
  * To deploy freeswitch, **not** mandatory, but it's easier to deploy using it and some other changes may be needed
  * sudo docker run -d --name freeswitch -p 5060:5060/tcp -p 5060:5060/udp -p 5080:5080/tcp -p 5080:5080/udp -p 8021:8021/tcp -p 7443:7443/tcp -p 60535-60635:60535-60635/udp -v \<PATH_TO_CONF\>:/usr/local/freeswitch/conf bettervoice/freeswitch-container:1.6.16
  * **Important** this docker image places freeswitch's executable in ```/usr/src/freeswitch``` but loads configuration files from ```/usr/local/freeswitch```, also the configuration files are placed in ```../conf/vanilla``` directory, instead of the default ```../conf``` directory.

## How to

1. Checkout the project :)
3. Change the ext-rtp-ip and ext-sip-ip parameters at ./conf/sip_profiles.xml depending on your needs (more instructions can be found in the file). 
3. Change the ips at the ./conf/autoload_modules/xml_curl.conf.xml and ./conf/vars.xml
    * On both files a ##CHANGE_ME## shows where to put the ips
    * On vars.xml we need to change the ip freeswitch will use as domain, for aws instances I used the instance public ip
    * On xml_curl.conf.xml we need to show where is the service that will provide the dialplan xml(if it's on the same address you can replace gateway-url's ##CHANGE_ME## with ```$${domain}```).
4. If using docker, start the freeswitch container using the command above. Remember to change \<PATH_TO_CONF> with the path to the conf folder of this project (/home/ubuntu/xml_curl_test/conf)
5. Start the dialplan webserver.
    * Go to the dialplan folder
    * Run ```node app.js```

Now it should be working, just register one of the default freeswitch users and try to make a call.

Look at ./dialplan/dialplan.xml to see what extensions are available to use.


