# PrivateServer

## Introduction 

The project is to create a private cloud system on a raspberry pi mounted with 4 Hard Drives. 

We are going to use as Hardware:

- 1 Raspberry Pi 4GB 
- 1 cooling HAT: https://fr.aliexpress.com/item/4000570741979.html
- 4 HHD 3.5" 1To: https://www.amazon.fr/gp/product/B0088PUEPK/
- FANTEC QB-35US3-6G: https://www.amazon.fr/gp/product/B00ORENYJE/
- 1 micro SD card : https://www.amazon.fr/gp/product/B073K14CVB/

We are going to use as Software : 

- docker
- nextcloud
- tailscale

## Set-up of the Raspberry Pi 

I choose to flash a micro sd card with Ubuntu 22.04 in order to configure my Raspberry Pi. You can find some tutorial online.
Once you went through the first configuration of the Pi ( not working headlessly for me) you can do the rest of the tutorial via ssh. 


### Docker 

Install docker :
````
sudo apt update
sudo apt install docker docker-compose
````

### Mount the Hard Drives

Before you can mount your external storage medium, you need to create a partition and format it. 

Find the location of the disks :
````
sudo fdisk -l
````
They should be at the bottom of the list, at the very bottom there should be a table if the external device already has partitions.
in either way, run : 
````
sudo gdisk /dev/sda
````
If partitions are present enter 'd' as command to delete old partitions. When it asks for command again press 'w' to saves the changes, then re-enter the previous command line to create new partitions.

If no partitions are present you can enter 'n' after it asked for command in order to create a new partition. 
Keep pressing enter to set the default parameters. 
When it asks for command again press 'w' to saves the changes.

Then enter this command to format the newly created partition so that the operating system can write data to the disk : 

````
sudo mkfs.ext4 /dev/sda1
````

Mount them to a newly created folder :
````
sudo mkdir /mnt/external-drive/disk1
sudo mount /dev/sda1 /mnt/external-drive/disk1
````
Make sure to replace the name of the folder by yours. 

If you want to unmount use : 

````
sudo umount /dev/sda1
````

Be aware that this mount is not permanent and need to be redone every time the Raspberry Pi is rebooted. 
To do an auto-mount of the external Hard drives follow this tutorial : https://www.linuxbabe.com/desktop-linux/how-to-automount-file-systems-on-linux.

Or, run this command : 

````
blkid
````
Copy the UUID of your storage device and add it at the end of the '/etc/fstab' file :
````
sudo nano /etc/fstab
PARTUUID=<-your-UUID-> /mnt/external-drive ext4 defaults 0 0
````
### Run the Tailscale docker

Install Tailscale on your raspberry pi :
````
curl -fsSL https://tailscale.com/install.sh | sh
````

Connects your device to Tailscale, and authenticates if needed : 
````
sudo reboot 
sudo tailscale up
````

Run your Tailscale docker : 
````
docker-compose -f docker-compose-tailscale.yaml up -d
````

Check your status with : 
````
tailscale status
````

Run the following command, it lets you share a local service securely within your tailnet. --bg is for backgroung.
(11000 is the port set to be used by Apache, cf docker-compose of nextcloud) 

````
tailscale serve --bg 11000
````

### Run the Nextcloud AIO docker command 


Run the Nextcloud docker 
````
docker-compose -f docker-compose.yaml up -d
````

### Access to your server 

You can access to your server on the adress 
````
https://ip.address.of.tailscale.ip.adress:8080
````





