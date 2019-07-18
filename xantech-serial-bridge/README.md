# Xantech Serial Bridge (Hass.io Add-On)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=WREP29UDAMB6G)

Exposes a REST interface that bridges to a serial connected multi-zone amplifier that supports the
Xantech RS232 serial control protocol.

## Required Hardware

* multi-zone amplifier or controller that supports to the Xantech RS232 serial protocol (see below)
* host machine with a serial cable or network serial adapter to physically connect to a Xantech supported multi-zone amplifier/controller
* host machine for executing the Docker container (e.g. [Raspberry Pi](https://www.raspberrypi.org/) running Home Assistant's [Hass.io](https://www.home-assistant.io/hassio/) hypervisor)

#### Supported Amplifiers/Controllers

| Manufacturer  | Model(s)                        | Supported |
| ------------- | --------------------------------|:---------:|
| Xantech       | MRAUDIO8X8 / MRAUDIO8X8m        | YES       |
|               | MRC88 / MRC88m                  | YES       |
|               | MX88 / MX88a / MX88ai / MX88vi  | YES       |
|               | MRAUDIO8X8 / MRAUDIO8X8m        | YES       |
|               | MRAUDIO4X4                      | NO        |
|               | MRC44 / MRC44CTL                | NO        |
| Monoprice     | MPR-SG6Z / 10761                | MAYBE *   |
| Dayton Audio  | DAX66                           | MAYBE *   |


* The [Monoprice MPR-SG6Z](https://www.monoprice.com/product?p_id=10761) and
  [Dayton Audio DAX66](https://www.parts-express.com/dayton-audio-dax66-6-source-6-room-distributed-whole-house-audio-system-with-keypads-25-wpc--300-585)
  appear to have licensed or copies the serial interface from Xantech. Both Monoprice
  and Dayton Audio use a version of the Xantech multi-zone controller protocol.

While Xantech/Monoprice/Daytona Audio amplifiers support expanding the number of zones by connecting two (or three)
amplifiers together, the Xantech Serial Bridge enables an "unlimited" number of amplifiers to be added
to a system and controlled via a REST interface. In this case, for each amplifier a separate Xantech
Serial Bridge should be instantiated with a separate serial cable connected to each amplifier.

## Installation

#### Install as a Docker Container

While this is called a Hass.io Add-on, that is merely semantic packaging around a Docker container,
which can also be executed directly.

```bash
docker build -t xantech-serial-bridge .
```

#### Install as a Hass.io Add-on

1. In the Hass.io "Add-On Store" on your Home Assistant server, add this repository URL:
<pre>
     https://github.com/rsnodgrass/hassio-addons
</pre>

2. Find the "Xantech Serial Bridge" in the list of add-ons and click Install

## Configuration

```json
{ 
   "zone_names": {
      "1": "Living Room",
      "2": "Kitchen",
      "3": "Master Bedroom",
      "4": "Master Bathroom",
      "5": "Home Theater",
      "6": "Kids Room",
      "7": "Garage",
      "8": "Patio"
   }
}
```

## REST Interface 

#### Command Line Interaction

Show details for zone 1:

```bash
curl http://localhost:5000/xantech/zones/1
```

Response:

```json
```

Mute zone 4:

```bash
curl -X POST http://localhost:5000/xantech/zones/4/mute/on
```

# TODO

* should this expose MQTT so that events from serial devices get propagated? (rather than polled)
   - or just optionally add broker support? (in addition to REST API) (broker:port)
* add documentation of all the API endpoints and link from here
* add support for a remote Global Cache iTach Flex IP/Wifi serial interface where the Xantech Serial Bridge can't physically be connected via serial to the amplifier
* ability to remote configure or rename zones/sources via the REST API

# See Also

* [Home Assistant integration for the Xantech Serial Bridge](https://github.com/rsnodgrass/hass-integrations/tree/master/custom_components/xantech_mza)
* [Monoprice RS232 serial protocol manual](doc/Monoprice-RS232-Manual.pdf)
* [Monoprice RS232 serial protocol control codes](doc/Monoprice-RS232-Control-Codes.pdf)
* [Monoprice mpr-6zhmaut-api NodeJS REST server](https://github.com/jnewland/mpr-6zhmaut-api)
* [Monoprice 10761 iOS and Apple Control control app](https://apps.apple.com/us/app/monoprice-whole-home-audio/id1168858624) (just as a reference, it does not use this bridge)