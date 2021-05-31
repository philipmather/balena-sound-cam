# balena-sound-cam
https://www.balena.io/blog/two-projects-one-device-turn-your-raspberry-pi-into-a-multitool/

```
cd ~/Projects/
git clone git@github.com:philipmather/balena-sound-cam.gi
cd balena-sound-cam/

git submodule add git@github.com:balenalabs/balena-sound.git
#git submodule add git@github.com:balenalabs/balena-cam.git
# In my case I want to track my own branch so...
git submodule add -b add_res_select git@github.com:philipmather/balena-cam.git

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

## Update submodules
```
git submodule foreach git pull origin master
```


# Develop locally
* burn dev version to img
```
sudo dd if=~/Downloads/balena/balena-cloud-balena-sound-cam-raspberrypi4-64-2.75.0+rev1-dev-v12.5.10.img of=/dev/sdb bs=1M conv=noerror,sync status=progress
sudo eject /dev/sdb
```
* boot it, wait for it to update, and become available, then switch it to local mode and ensure mDNS is allowed allowed through your workstations firewall.
* Balena scan should now find it...
```
[matherp@localhost balena-cam]$ sudo /usr/local/bin/balena scan
[sudo] password for matherp: 
Scanning for local balenaOS devices... Reporting scan results
- 
  host:      balena-3.local
  address:   192.168.0.37
  osVariant: production
- 
  host:          balena-2.local
  address:       192.168.0.36
  osVariant:     development
  dockerInfo: 
    Containers:        1
    ContainersRunning: 1
    ContainersPaused:  0
    ContainersStopped: 0
    Images:            10
    Driver:            overlay2
    SystemTime:        2021-05-31T17:47:11.540130235Z
    KernelVersion:     5.4.83-v8
    OperatingSystem:   balenaOS 2.75.0+rev1
    Architecture:      aarch64
  dockerVersion: 
    Version:    19.03.18
    ApiVersion: 1.40
```
* You will need to pull all the submodules down locally now rather then just referencing them in git
```
git submodule update --init --recursive
```


#Issues
##OSes
* Redhat firewalld has mdns closed on the "default" zone, switch to "home" or similar zone or explicitly allow it for balena scan to work.

##Browsers
* Fix chrome to work with WebRTC https://github.com/balenalabs/balena-cam/pull/62/files

##Rpi 3
Remove DT_OVERLAY variable

##Rpi 4
Didn't have to remove the dt_overlay thing but did have to set the projects environment variable to AUDIO_OUTPUT : RPI_HEADPHONES because it was trying to use the mic on the webcam

#Balena-cam development
https://opensource.com/article/19/7/create-pull-request-github
Forked balena-cam to https://github.com/philipmather/balena-sound-cam
```
[matherp@localhost Projects]$ git clone git@github.com:philipmather/balena-cam.git
Cloning into 'balena-cam'...
remote: Enumerating objects: 796, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 796 (delta 2), reused 10 (delta 0), pack-reused 777
Receiving objects: 100% (796/796), 4.70 MiB | 5.29 MiB/s, done.
Resolving deltas: 100% (362/362), done.
[matherp@localhost Projects]$ git checkout -b add_res_select
fatal: not a git repository (or any parent up to mount point /)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).
[matherp@localhost Projects]$ cd balena-sound-cam/
[matherp@localhost balena-sound-cam]$ cd ..
[matherp@localhost Projects]$ cd balena-cam/
[matherp@localhost balena-cam]$ git checkout -b add_res_select
Switched to a new branch 'add_res_select'
[matherp@localhost balena-cam]$ git remote add upstream git@github.com:balenalabs/balena-cam.git


$ git add test
$ git commit -S -m "Adding a test file to new_branch"
$ git push -u origin new_branch
```
Once you push the changes to your repo, the Compare & pull request button will appear in GitHub.


# Other links
https://linuxhint.com/dd_command_linux/
https://gist.github.com/myusuf3/7f645819ded92bda6677
https://stackoverflow.com/questions/1777854/how-can-i-specify-a-branch-tag-when-adding-a-git-submodule
