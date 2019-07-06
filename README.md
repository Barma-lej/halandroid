<a href="https://www.buymeacoffee.com/barma" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>

# Worx Landroid package for Home Assistant
Worx Landroid package for Home Assistant based on Landroid Bridge by Heiner virtualzone

![Landroid](halandroid1.png)

![Landroid](/www/mower/halandroid.png)

## Landroid Bridge installation
For the package to work, you need to install Landroid Bridge: https://github.com/virtualzone/landroid-bridge

I have a Raspberry Pi 3 Model B with Raspbain Stretch and Home Assistant in folder ~/pi/.homeassistant

#### Update Node
```bash
cd ~
sudo apt update
sudo apt upgrade
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```
Then i build Landroid Bridge
```bash
git clone https://github.com/virtualzone/landroid-bridge.git
cd landroid-bridge
npm install
```
I got a sqlite3 error here :( and need to install it

```bash
sudo apt-get install libsqlite3-dev
npm install sqlite3 --build-from-source --sqlite=/usr
npm run grunt
```

#### Setting up MQTT
To connect to an MQTT broker without any authentication, please modify your **~/landroid-bridge/config.json**

My **config.json** file without personal info is here:

```json
{
    "http": {
        "port": 3000
    },
    "landroid-s": {
        "enable": true,
        "email": "EMAIL_FOR_MYWORX",
        "pwd": "PASSWORD_FOR_MYWORX",
        "dev_sel": 0
    },
    "mqtt": {
        "enable": true,
        "url": "mqtt://MQTT_USER:MQTT_PASSWORD@localhost",
        "topic": "landroid",
        "//clientId": "optional",
        "//caFile": "./optional_path_to_ca_file.crt",
        "//keyFile": "./optional_path_to_key_file.key",
        "//certFile": "./optional_path_to_cert_file.crt",
        "//allowSelfSigned": true
    },
    "logLevel": "info",
    "scheduler": {
        "enable": true,
        "cron": false,
        "weather": {
            "provider": "darksky",
            "apiKey": "DARKSKY_API_KEY",
            "latitude": 0.481478,
            "longitude": 0.742742
        },
        "db": "./scheduler.db",
        "earliestStart": 9,
        "latestStop": 21,
        "startEarly": false,
        "offDays": 0,
        "squareMeters": 200,
        "perHour": 100,
        "mowTime": 90,
        "chargeTime": 75,
        "daysForTotalCut": 0,
        "rainDelay": 240,
        "threshold": 30
    }
}
```

See here for details: https://github.com/virtualzone/landroid-bridge#setting-up-mqtt

#### Set up a systemctl script to start the bridge on system startup:

Adjust the username and paths (path to current node too!!!) in the file **~/landroid-bridge/systemctl-script/landroid-bridge.service**, and copy it to /lib/systemd/system/

```bash
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
3. Add to your Lovelace config content from file **add_to_lovelace.yaml** ([How? See here](/help/work_with_lovelace.md))
4. If you need, translate strings in files **worx_landroid.yaml** section _customize_ and **add_to_lovelace.yaml** from German and Russian
5. If you use a Google Assistant, then use switch **landroid_mowing**:
```yaml
  switch.landroid_mowing:
    name: Mähroboter
    room: Garten
    expose: true
```
<a href="https://www.buymeacoffee.com/barma" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/white_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>

#### _Enjoy_

#### _Удачи_:)
