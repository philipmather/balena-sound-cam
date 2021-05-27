# balena-sound-cam
https://www.balena.io/blog/two-projects-one-device-turn-your-raspberry-pi-into-a-multitool/

```
cd ~/Projects/
git clone git@github.com:philipmather/balena-sound-cam.gi
cd balena-sound-cam/
git submodule add git@github.com:balenalabs/balena-sound.git
git submodule add git@github.com:balenalabs/balena-cam.git
cat balena-sound/docker-compose.yml balena-cam/docker-compose.yml  > ./docker-compose.yml
vi ./docker-compose.yml
# Only changed build paths

 pushd ~/Downloads/
~/Downloads ~/Projects/balena-sound-cam
unzip ./balena-cloud-balena-sound-cam-raspberrypi3-64-2.65.0+rev1-v12.2.11.img.zip 

Archive:  ./balena-cloud-balena-sound-cam-raspberrypi3-64-2.65.0+rev1-v12.2.11.img.zip
  inflating: balena-cloud-balena-sound-cam-raspberrypi3-64-2.65.0+rev1-v12.2.11.img  

sudo dd bs=1M if=./balena-cloud-balena-sound-cam-raspberrypi3-64-2.65.0+rev1-v12.2.11.img of=/dev/sdb
892+0 records in
892+0 records out
935329792 bytes (935 MB, 892 MiB) copied, 97.9314 s, 9.6 MB/s

sudo eject /dev/sdb
popd
~/Projects/balena-sound-cam
```

https://github.com/balenalabs/balena-cam/pull/62/files
Didn't have to remove the dt_overlay thing but did have to set the projects environment variable to AUDIO_OUTPUT : RPI_HEADPHONES because it was trying to use the mic on the webcam
