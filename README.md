# Worx Landroid package for Home Assistant
Worx Landroid package for Home Assistant based on Landroid Bridge by Heiner virtualzone

![Landroid](/www/mower/halandroid.png)

## Landroid Bridge installation
For the package to work, you need to install Landroid Bridge: https://github.com/virtualzone/landroid-bridge

I have a Raspberry Pi 3 Model B with Raspbain Stretch and Home Assistant in folder ~/pi/.homeassistant

#### Update Node
```ssh
cd ~
sudo apt update
sudo apt upgrade
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```
Then i build Landroid Bridge
```ssh
git clone https://github.com/virtualzone/landroid-bridge.git
cd landroid-bridge
npm install
```
I got a sqlite3 error here :( and need to install it

```
sudo apt-get install libsqlite3-dev
npm install sqlite3 --build-from-source --sqlite=/usr
npm run grunt
```
#### Set up a systemctl script to start the bridge on system startup:

Adjust the username and paths (path to current node too!!!) in the file, and copy it to /lib/systemd/system/

See here for details: https://github.com/virtualzone/landroid-bridge#setting-up-mqtt

```
sudo cp landroid-bridge /lib/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable landroid-bridge.service
sudo systemctl start landroid-bridge.service
```

## Home Assistant

1. Create folder named **packages** in your homeassistant folder
1. Copy to your folder **packages** file **worx_landroid.yaml** from  folder **packages**
1. In configuration.yaml add string packages: _!include_dir_named packages_:
```yaml
homeassistant:
  packages: !include_dir_named packages
```
