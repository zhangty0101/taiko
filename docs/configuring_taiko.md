---
layout: page.njk
---

Listed below are instructions on how to modify Taiko's default paramaters
for specific uses cases

## Using environment variables

Taiko lets you specify certain Environment variables to customise its behaviour

* `TAIKO_SKIP_CHROMIUM_DOWNLOAD` - set to `true` to skip downloading chromium
* `TAIKO_HIGHLIGHT_ON_ACTION` - set to `false` to turn off highlighting the element on action
* `TAIKO_CHROMIUM_URL` - set to host url of mirror to download chromium
* `TAIKO_BROWSER_PATH` - set to launch browser from different location
* `TAIKO_BROWSER_ARGS` - set ',' separated browser command line switches to launch browser with extra args
* `TAIKO_EMULATE_DEVICE` - set to device model to emulate device view port
* `TAIKO_EMULATE_NETWORK` - set to the network type for Taiko to simulate. Available options are GPRS, Regular2G, Good2G, Regular3G, Good3G, Regular4G, DSL, WiFi, Offline
* `TAIKO_PLUGIN` - set to the plugin which you want Taiko to load. Takes comma separated values.