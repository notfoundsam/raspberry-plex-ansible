## About project

Simple way to install plex on your raspberry pi and mount connected USB drive.

### Requirements

- Ubuntu
- [Raspberry Pi Imager](https://www.raspberrypi.org/software/)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Raspberry pi 3/4 with SD card
- USB drive

### Preparation

1. Download Raspberry Pi OS Lite
2. In Raspberry Pi Imager choose use cusstom and select downloaded image. Choose your SD card and click write.
3. Go to the boot volume on SD card and create ssh file `touch ssh`
4. Generete private and public key with `cd ~/.ssh && ssh-gen` and set name for keypair as raspberry for example.
5. Go to the rootfs volume on SD card and create .ssh folder `mkdir home/pi/.ssh`.
6. Put your public ssh key into authorized_keys file `cp ~/.ssh/raspberry.pub home/pi/.ssh/authorized_keys`
7. Format USB drive to NTFS and set label `usb_750g` for example, we will be use it later.
8. Connect Ethernet, USB drive to your PI.
9. Set up static IP on your pi [read this article](https://www.raspberrypi.org/documentation/configuration/tcpip/) or set up permanent IP by MAC address on your DHCP server.

### Installing plex

1. In `hosts.ini` file replace IP with your static IP.
2. In `group_vars/all.yml` file replace variables with yours values.
3. Run `ansible-playbook plex.yml` to install plex media server.
4. Run `ansible-playbook usb-volume.yml` to mount your USB drive.
5. Is everything was OK open `your_ip:32400/web` and setup plex.

### Install samba server

Coming soon...
