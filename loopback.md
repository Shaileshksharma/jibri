#### for Centos 7, module already compiled with kernel, so just run:
```
# configure 5 capture/playback interfaces
echo "options snd-aloop enable=1,1,1,1,1 index=0,1,2,3,4" > /etc/modprobe.d/asound.conf
# setup autoload the module
echo "snd_aloop" > /etc/modules-load.d/snd_aloop.conf
# load the module
modprobe snd-aloop
# check that the module is loaded
lsmod | grep snd_aloop
