## About project

Simple way to install plex on your raspberry pi and mount connected USB drive. You can also set up samba server and install transmission for download torrents.
Or set up pi as file storage by installing only usb and samba. If you have two pi you can separate torrents and plex server.

### Requirements

- Ubuntu (or Windows with bash ubuntu)
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
8. Connect Ethernet, USB drive to your PI an power up.
9. Connect via ssh to pi `ssh -i cp ~/.ssh/raspberry pi@your_ip` and set up static IP on your pi [read this article](https://www.raspberrypi.org/documentation/configuration/tcpip/) or set up permanent IP by MAC address on your DHCP server. Run on pi `cat /sys/class/net/eth0/address` to get MAC address.

### Installing plex

1. In `hosts.ini` file replace IP with your static IP.
2. In `group_vars/all.yml` file replace `ansible_user`, `ansible_ssh_private_key_file`, `host_name` variables with yours values. Replace `usb_volume_label` with value from step 7 of preparation.
3. Run `ansible-playbook plex.yml` to install plex media server. Your pi will be rebooted in the end.
4. Run `ansible-playbook usb-volume.yml` to mount your USB drive.
5. Is everything was OK open `your_ip:32400/web` and setup plex. At first access it can take longer. Try access again if it shows some errors.

### Installing samba server

Run `ansible-playbook samba.yml` to install and set up. It will share your USB drive. In Windows access by `\\your_ip`.

### Installing transmission for torrents

1. In `group_vars/all.yml` file replace `transmission_username`, `transmission_password` with what you want and replace home network in `transmission_white_list`.
2. Run `ansible-playbook transmission.yml` to install and set up. It will create download folder in your USB drive and set up as default for downloads.
3. Access to transmission web `your_ip:9091/transmission`.
