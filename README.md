Docker and config files for home server

# Server Notes

Domain records are configured to point at localhost, nothing is publicly accessible 

## Current Services:

## Caddy
[Caddy](https://github.com/caddyserver/caddy) - Reverse proxy

## Dozzle
[Dozzle](https://github.com/amir20/dozzle) - Sometimes handy to look at logs without ssh

## Flame
[Flame](https://github.com/pawelmalak/flame) - Custom dashboard page in combo with https://github.com/methodgrab/firefox-custom-new-tab-page

## Grafana 
[Grafana](https://github.com/grafana/grafana) - Basic monitoring

## Home Assistant
[Home Assitant](https://github.com/home-assistant) - Local control of smart devices

## Linkding
[LinkDing](https://github.com/sissbruecker/linkding) - RIP pocket

## Mealie
[Mealie](https://github.com/mealie-recipes/mealie) - Recipe manager, current favorite

## Tandoor
[Tandoor](https://github.com/TandoorRecipes/recipes) - Previous recipe manager

## Silver Bullet
[Silver Bullet](https://github.com/silverbulletmd/silverbullet) - Shared markdown notes

## Wud
[What's Up Docker](https://github.com/getwud/wud) - Hacky way of getting a local cicd server. Github actions buidls docker container, this pulls updated container in without having to expose any connection

## Zigbee2mqtt
[Zigbee2Mqtt](https://github.com/Koenkk/zigbee2mqtt) - Allow zigbee devices to communicate with Home Assistant

## Jellyfin
## Portainer

## Setting up server

- set static ip on router
- edit /etc/network/interfaces
- docker volume create caddy_data
- docker network create caddy_net

- increase buffer sizes
    - sudo sysctl -w net.core.rmem_max=2500000
    - sudo sysctl -w net.core.wmem_max=2500000

copy media over (from old server) 
- scp -r /abs/path/to/media josh@ip:/abs/path/to/dest 

See current caddy config file `docker exec -w /etc/caddy $caddy_container_id cat Caddyfile`


### Replacing battery

Replacing the CMOS battery clears some important BIOS settings. Here is a list of settings to check/ update if necessary

Bios -> Advanced:
    System Options:
        - Uncheck configure storage controller for RAID
        - uncheck configure for intel optane
    - Secure Boot Configuration:
        - legacy disable secure boot enable
    Boot Options:
        - uncheck fast boot
        - uncheck network PXE boot
        - after power loss -> power on
        - UEFI Boot order (debian after creating it)

After fixing BIOS settings, debian would not boot, giving a 3f0 hard disk error. In the boot menu, selecting boot from file and manually choosing the grub/shim efi file enabled debian to boot correctly. To fix it, after booting into debian: 

efibootmgr -v to see boot options. shim.efi path was present and enabled, but not in the boot list. Reference that path for -l parameter. For -d, find drive with `lsblk` to find the main drive, then `sudo fdisk -l /dev/nvme0n1` (nvme0n1 being the min drive in this case)
Creating new boot option

`sudo efibootmgr -c -p 1 -d /dev/nvme0n1 -L "debian" -l '\EFI\debian\shimx64.efi'`

### Tandoor
failing to come up, logs eventually showed static files missing 
`docker volume rm tandoor_staticfiles`

### Resources

https://github.com/DoTheEvo/selfhosted-apps-docker/tree/master

### TODO
- [ ] change CF token
- [ ] jellyfin
- [x] deconz
- [ ] case insensitive autocomplete
- [ ] add deconz to homepage

Later
- [ ] remove immich 
- [ ] remove ghost
- [ ] filebrowser (or similar)
- [ ] dockage? 
- [ ] chat client? 

