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

Find the location of the disks :
````
sudo fdisk -l
````

Mount them to a newly created folder :
````
sudo mkdir /mtn/external
sudo mount /dev/sda1 /mnt/external
````
Make sure to replace the name of the folder by yours. 

Be aware that this mount is not permanent and need to be redone every time the Raspberry Pi is rebooted. 
To do an auto-mount of the external Hard drives follow this tutorial : https://www.linuxbabe.com/desktop-linux/how-to-automount-file-systems-on-linux


