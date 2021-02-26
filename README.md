# Helium Hotspot EU version

This project deploys a LoRaWAN Helium Hotspot with European configuration based on SX1301 LoRa chip.

## Getting started

This project runs on Raspberry Pi 3/4 on balenaOS 64 bits with RAK2245 or RAK7324 concentrator (SX1301).

You will need a [balenaCloud free account](https://dashboard.balena-cloud.com/) and [balenaEtcher](https://balena.io/etcher) for flashing the SD card.

## Deploy the code

Deploy the code with this button

[![](https://www.balena.io/deploy.png)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/PastaGringo/balenaos-helium-gtw)


## Modify the code

If you want to try you need to:

1) fork my repo to your own github account : https://github.com/PastaGringo/balenaos-helium-gtw
2) inside the readme change the `repoUrl=https://github.com/PastaGringo/balenaos-helium-gtw` by `repoUrl=https://github.com/YourUsername/balenaos-helium-gtw`
3) modify the docker-compose file to configure lora service based on your needs:
```bash
      args:
        - LORA_REGION=EU868
        - LORA_UDP=1680 #default "1680"
        #- SPI_SPEED=8000000 #default 2000000
        #- PIN_RESET=11
        - MINER_URL=helium-miner #default "helium-miner"
```
4) modify the docker-compose file to configure helium-miner service based on your needs:
```bash
    environment:
      - 'REGION_OVERRIDE=EU868'
```
4) click on deploy from your own repo page
5) create the app from CloudBalena, download the image, flash the image and insert the SD card into your device
6) boot the device


## How to update the miner

To update the Helium miner, click this button and choose "deploy to existing application" (automated miner updates coming soon ;))


## How to backup the swarm key

1. Open an SSH session to the "host-os"
2. Type this command and keep note of the (YOUR INSTANCE)_miner-data information: 
      ls /var/lib/docker/volumes
3. Type this command to get a link to download the swarm key (note to replace the YOUR INSTANCE part with the container number that you got from the previous command) 
      curl -F "file=@/var/lib/docker/volumes/(YOUR INSTANCE)_miner-data/_data/miner/swarm_key" https://file.io
4. Use the outputted file.io link to securely download your swarm key. The link only works one time.


## How to restore the swarm key

1. Open an SSH session to the "host-os"
2. Type this command and keep note of the (YOUR INSTANCE)_miner-data information: 
      ls /var/lib/docker/volumes
3. Navigate to where the swarm_key is stored
      cd /var/lib/docker/volumes/(YOUR INSTANCE)_miner-data/_data/miner/
4. Remove the original swarm_key file
      rm swarm_key
5. Upload your swarm_key that you wish to restore onto file.io and do
      curl -LJO [FILE.IO UPLOAD LINK]
6. Reboot miner and you will see it restored :)
