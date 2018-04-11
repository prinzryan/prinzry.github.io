


*15 December 2017*
### Docker? 
Docker is an open source container base technology. It runs in a container, which is a type of virtualization where the host kernel is used instead of a hypervisor that emulates hardware and then a VM runs its own kernel. Docker container shares all the resources of the host machine. Docker works kind of like git. You can have a container image and then you can commit changes to and then use docker to deploy the changes to all of your docker containers by doing a docker pull <container> on each of your hosts.


*09 January 2018*
### Why Docker?
Docker has docker files that describe very simply how the container will work. You can make a Docker file and docker will build you a nice container. Docker is great at setting up a local development environment because it easily adds the running process without duplicating the virtualized resource. 

## What is Container?
Containers are like VMs, and they separates the resources that an application has access to such as (all those libraries, the code and other dependencies). You don’t need to worry about whether your container is going to be shipped to a RedHat machine, an Ubuntu machine, or a CentOS VM image; as long as it has Docker on it, it’ll be good to go.


*19 January 2018*
### Installing Docker
When installing Docker check the requirements to see if it is available on your computer. 
https://docs.docker.com/datacenter/ucp/1.1/installation/system-requirements/ 

Docker is supported by two editions; Community Editions and Enterprise Editions.

-See more Information for CE. https://www.docker.com/community-edition 
-See more Information for EE. https://www.docker.com/enterprise-edition 

Docker also supports Windows, Mac OS, Linux’s platforms. Choose what platform you want to install. Link below to follow installing
https://docs.docker.com/install/ 


*27 January 2018*
### Installing Linux 
The hardware you need to follow along is a *Raspberry Pi 1* or *2* and one SD card. 
If you are going to use a Raspberry 2 you will have to use a microSD card - otherwise a normal SD card is sufficient. We recommend a size of at least 4 GB. 
Etcher is typically the easiest option for most users to write images to SD cards, so it is a good place to start.
```
•	You should run df –h to see which devices are currently mounted.
•	If your computer has a slot for SD cards, insert the card. If not, insert the card into an SD card reader, then connect the reader to your computer. 
•	Run df –h again. The new device that has appeared is your SD card. If no device appears, then your system is not auto mounting devices. To display the most recent system messages, which should contain information on the naming of the SD card device use the command dmesg | tail. 
•	The left column of the results from df -h command gives the device name of your SD card. It will show like /dev/mmclk0p1 or /dev/sdX1, X is the lower case to show the device. The last part (p1 or 1 respectively) is the partition number. You want to write to the whole SD card, not just one partition. You have to remove that section from the name. You should see something like /dev/mmcblk0 or /dev/sdX as the device name for the whole SD card.
•	You must have noted the device name now, you need to unmount it so that files can't be read or written to the SD card while you are copying over the SD image. 
•	Run umount /dev/sdX1, replacing sdX1 with whatever your SD card's device name is, including the partition number.
•	If your SD card’s keeps showing up a lot in the output of “df”, it means that the card has multiple partitions. You need to unmount all of the partitions.
``` 

# Copying the Image to the SD Card
```
•	In a terminal window, write the image to the card with the command below, making sure you replace the input file if= argument with the path to your .img file, and the /dev/sdX in the output file of= argument with the correct device name. If you provide the wrong device name you will lose all the data on the hard drive, that is why this is very important. You have to make sure that the device name is the name of the whole SD card as described above, not just a partition.
 For example: sdd, not sdds1 or sddp1, and mmcblk0, not mmcblk0p1.

dd bs=4M if=2017-09-07-raspbian-stretch.img of=/dev/sdX conv=fsync
•	Please note that block size set to 4M will work most of the time. If not, try 1M, although this will take considerably 
```

# Checking the Image Copy Progress
```
•	The command dd   won’t give you any information about the progress, it may appear like it’s frozen. It takes a couple of minutes to finish writing to the card. It may blink during the write process if your card reader has an LED.
•	You can run the dd command with the status option, to see the progress of the copy operation. 

dd bs=4M if=2017-09-07-raspbian-stretch.img of=/dev/sdX status=progress conv=fsync
 
•	Use the dcfldd command instead, which will give a progress report showing how much has been written. Another method is to send a USR1 signal to dd, which will let it print status information. Find out the PID of dd by using pgrep -l dd or ps a | grep dd. Then use kill -USR1 PID to send the USR1 signal to dd. 
```

# Checking whether the Image was correct written to SD Card
```
•	After dd has finished copying, you can check what has been written to the SD card by dd-ing from the card back to another image on your hard disk; truncating the new image to the same size as the original; and then running diff (or md5sum) on those two images.
•	If the SD card is bigger than the original image size, dd will make a copy of the whole card. We must therefore truncate the new image to the size of the original image. Make sure you replace the input file if= argument with the correct device name. diff should report that the files are identical. 

dd bs=4M if=/dev/sdX of=from-sd-card.img
truncate --reference 2017-09-07-raspbian-stretch.img from-sd-card.img
diff -s from-sd-card.img 2017-09-07-raspbian-stretch.img 

•	Run sync. This will ensure the write cache is flushed and that it is safe to unmount your SD card. 
•	Remove the SD card from the card reader.
```

