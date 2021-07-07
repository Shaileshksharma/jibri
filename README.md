Jibir Build for FB streaming....
How to run multipal Docker instance of Jibri on Same host
1. Configure .env file and change username and password
2. Configure jibri.yml
3. docker-compose -f jibri.yml up -d --scale jibri=2
4. Now login to each instance and change .asoundrc file in /home/jibri. Change the loopback number as below....

pcm.amix {
  type dmix
  ipc_key 219345
  slave.pcm "hw:Loopback_2,0,0"
}

pcm.asnoop {
  type dsnoop
  ipc_key 219346
  slave.pcm "hw:Loopback_3,1,0"
}

pcm.aduplex {
  type asym
  playback.pcm "amix"
  capture.pcm "asnoop"
}

pcm.bmix {
  type dmix
  ipc_key 219347
  slave.pcm "hw:Loopback_3,0,0"
}

pcm.bsnoop {
  type dsnoop
  ipc_key 219348
  slave.pcm "hw:Loopback_2,1,0"
}

pcm.bduplex {
  type asym
  playback.pcm "bmix"
  capture.pcm "bsnoop"
}

pcm.pjsua {
  type plug
  slave.pcm "bduplex"
}

pcm.!default {
  type plug
  slave.pcm "aduplex"
}


Change it for each container......
5. Some OS may need to restart docker container for changes. You can do it by below command

docker restart container_name

6. You may need to load loopback devices...

In Ubuntu 18.04 install alsa lib 

apt-get install alsa

Now add below line loopback to /etc/modprobe.d/alsa-loopback.conf

options snd-aloop enable=1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1 index=0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16.

Now reload module using below command

rmmod snd_aloop -f

modprobe snd_aloop

To confirm loopback run aplay -l.... You will see all the loopback

