# piSchoolBell
The goal of this project is to run a Raspberry Pi as a controller for a school bell. It will make the school bell ring at the right times taking the calendar into consideration. Also there will be a LCD screen to visualize some things.

Installing OS
=============================
Download Raspbian Stretch Lite from https://www.raspberrypi.org/downloads/raspbian/  
Choose the Light zip-file  

Cd to where your download is  
$ unzip 2017-11-29-raspbian-stretch-lite.zip  

Insert SD-card and find out drive letter  
$ dmesg  
For example /dev/mmcblk0 or /dev/sdb  

Unmount if mounted  
$ umount /dev/mmcblk0p1  

Write image to SD-card  
$ sudo dd bs=4M if=2017-11-29-raspbian-stretch-lite.img of=/dev/mmcblk0 conv=fsync status=progress 

Remove SD-card and insert it again to make new partitons visible     

Mount the first partition  
$ sudo mount /dev/mmcblk0p1 /mnt/tmp  

Write empty file to boot partition to enable ssh at boot  
$ sudo touch /mnt/tmp/ssh  

Unmount  
$ sudo umount /mnt/tmp  

Remove SD-card and insert it a Rpi connected to your local network and boot it up 

Rpi configuration
-----------------------------
Connect to Rpi via ssh  
Login with user: pi and password:raspberry 

Update  
$ sudo apt-get update && sudo apt-get upgrade  

Configure  
$ sudo raspi-config   
1		Change password  
2 N1	Change hostname 
2 N2	Set SSID and passphrase   
4 I1	Set locales  
4 I2	Set time zone  
4 I3	Choose keyboard layout    
4 I4	Set wifi country  
7 A1	Expand file system to use whole SD-card  
7 A3	Set memory split to 16  
Reboot to set new options  

Installation
=============================
Later i will probably make an installation script, but for now this is how it goes...  

Install git
-----------------------------
$ sudo apt-get install git  

Clone repository
-----------------------------
$ cd /home/pi  
$ git clone https://github.com/jonsag/piSchoolBell.git  

Run install script
-----------------------------
$ cd /home/pi/piSchoolBell  
$ sudo ./install  

Initialize mysql
-----------------------------
$ sudo mysql -u root -p  
Use the same password as pi login  
Quit with exit  

Create database and insert initial data
-----------------------------
$ sudo ./mysql-setup.sh  

Add test data, if wanted  
$ sudo mysql -u root -p piSchoolBell < mysql-test-data.sql  


 

Below is only for my own convieniance during programming of this project
-----------------------------

rsync -raci ~/Documents/EclipseWorkspace/piSchoolBell/* pi@192.168.10.44:/home/pi/bin/piSchoolBell/

rsync -raci ~/Documents/EclipseWorkspace/piSchoolBell/www/* pi@192.168.10.44:/var/www/piSchoolBell/
ssh pi@192.168.10.44 'sudo chmod 755 -R /var/www/piSchoolBell'
ssh pi@192.168.10.44 'sudo chown -R pi:www-data /var/www/piSchoolBell'




