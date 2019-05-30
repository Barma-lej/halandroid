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

1. Copy folders **www** and **packages** with all content in your homeassistant folder
2. In configuration.yaml add string _packages: !include_dir_named packages_:
```yaml
homeassistant:
  packages: !include_dir_named packages
```
3. Add to your Lovelace config content from file **add_to_lovelace.yaml**
4. If do you need translate strings in files **worx_landroid.yaml** section _customize_ and **add_to_lovelace.yaml** from German and Russian

#### _Enjoy_

#### _Удачи_:)
