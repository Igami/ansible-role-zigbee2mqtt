# Ansible Role `zigbee2mqtt`

## Description

This role allows installing and configuring [zigbee2mqtt](https://www.zigbee2mqtt.io/) on Raspberry Pi
running stock Raspbian lite or full. Should also work on other Debian distributions.

The role is based on [igami.zigbee2mqtt](https://github.com/Igami/ansible-role-zigbee2mqtt)

## Requirements

Raspberry Pi with SSH enabled and CC2531 USB sniffer.  

If installing on a fresh 'headless' Raspberry Pi server, add an empty file named 'ssh' to the boot
directory of the SD card to enable remote SSH access.

## Role Variables

| Variable                        | Type           | Default                                     | Comments                                                                                                                                                                                                                                    |
|---------------------------------|----------------|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| zigbee_user                     | string         | `zigbee`                                    | User running `zigbe2mqtt`.                                                                                                                                                                                                                  |
| zigbee_user_groups              | string         | `tty,dialout`                               | Necessary groups for zigbee user to be able to access the serial port.                                                                                                                                                                      |
| zigbee_user_append              | boolean        | `false`                                     | If `true`, add the user to the groups specified in `zigbee_user_groups`.<br>If `false`, user will only be added to the groups specified in `zigbee_user_groups`, removing them from all other groups.                                       |
| zigbee_dir                      | string         | `/opt/zigbee2mqtt`                          | Installation directory for `zigbe2mqtt`.                                                                                                                                                                                                    |
| zigbee_repository               | string         | `https://github.com/Koenkk/zigbee2mqtt.git` | URL of the `zigbe2mqtt` git repository used for the installation.                                                                                                                                                                           |
| zigbee_version                  | string         | `HEAD`                                      | Version to install (should be a commit hash, branch or tag name.                                                                                                                                                                            |
| zigbee_permit_join              | boolean        | `false`                                     | Allow new devices to join.                                                                                                                                                                                                                  |
| zigbee_mqtt                     | object         |                                             | MQTT settings (see [zigbee_mqtt](#zigbee_mqtt)).                                                                                                                                                                                            |
| zigbee_advanced                 | object         |                                             | Advanced zigbe2mqtt settings to configure the adapter and other things (see [zigbee_advanced](#zigbee_advanced)).                                                                                                                           |
| zigbee_serial                   | object         |                                             | Serial port settings (see [zigbee_serial](#zigbee_serial)).                                                                                                                                                                                 |
| zigbee_frontend                 | object/boolean |                                             | If `false`, the frontend will be disabled. An object will configure the frontend settings (see [zigbee_frontend](#zigbee_frontend)).                                                                                                        |
| zigbee_ota                      | object         |                                             | OTA device firmware update settings (see [zigbee_ota](#zigbee_ota)).                                                                                                                                                                        |
| zigbee_devices                  | object         |                                             | Device settings (see the [configuration guide for devices and groups](https://www.zigbee2mqtt.io/guide/configuration/devices-groups.html#common-device-options)).                                                                           |
| zigbee_device_options           | object         |                                             | Device options settings (see the [configuration guide for devices and groups](https://www.zigbee2mqtt.io/guide/configuration/devices-groups.html#default-values)).                                                                          |
| zigbee_groups                   | object         |                                             | Group settings (see the [configuration guide for devices and groups](https://www.zigbee2mqtt.io/guide/configuration/devices-groups.html#groups)).                                                                                           |
| zigbee_blocklist                | object         |                                             | Device blocklist (see the [configuration guide for device blocklist / passlist](https://www.zigbee2mqtt.io/guide/configuration/block-pass-list.html)).                                                                                      |
| zigbee_passlist                 | object         |                                             | Device passlist (see the [configuration guide for device blocklist / passlist](https://www.zigbee2mqtt.io/guide/configuration/block-pass-list.html)).                                                                                       |
| zigbee_external_converters      | object         |                                             | External converter settings (see the [configuration guide for external converters](https://www.zigbee2mqtt.io/guide/configuration/more-config-options.html#external-converters)).                                                           |
| zigbee_map_options              | object         |                                             | Network map settings (see the [configuration guide for network map](https://www.zigbee2mqtt.io/guide/configuration/more-config-options.html#network-map)).                                                                                  |
| zigbee_availability             | object         |                                             | Device availability feature settings (see the [configuration guide for device availability](https://www.zigbee2mqtt.io/guide/configuration/device-availability.html)).<br>If not defined, the device availability feature will be disabled. |
| zigbee_homeassistant            | object         |                                             | Home assistant integration settings (see the [configuration guide for home assistant integration](https://www.zigbee2mqtt.io/guide/configuration/homeassistant.html)).<br>If not defined, the home assistant integration will be disabled.  |
| zigbee_generate_new_network_key | boolean        | `false`                                     | If `true` force to generate a new network key (only if no network key is specified in `zigbee_advanced.network_key`).                                                                                                                       |

### zigbee_mqtt

Details of all settings can be found in the [configuration guide for MQTT](https://www.zigbee2mqtt.io/guide/configuration/mqtt.html).

| Variable   | Type         | Default            | Comments                                     |
|------------|--------------|--------------------|----------------------------------------------|
| base_topic | string       | `zigbee2mqtt`      | Base topic for `zigbe2mqtt` MQTT messages.   |
| server     | string       | `mqtt://localhost` | URL to connect to the the MQTT server.       |
| user       | string       |                    | User name to connect to the the MQTT server. |
| password   | string       |                    | Password to connect to the the MQTT server.  |

### zigbee_advanced

Details of all settings can be found in:
- the [configuration guide for adapter settings](https://www.zigbee2mqtt.io/guide/configuration/adapter-settings.html)
- the [configuration guide for zigbee network](https://www.zigbee2mqtt.io/guide/configuration/zigbee-network.html#network-config)
- the [configuration guide for logging](https://www.zigbee2mqtt.io/guide/configuration/logging.html)

| Variable     | Type     | Default                                            | Comments                                                                                                                            |
|--------------|----------|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| pan_id       | hex      | `0x1a62`                                           | ZigBee pan ID.                                                                                                                      |
| ext_pan_id   | array    | `[0xDD, 0xDD, 0xDD, 0xDD, 0xDD, 0xDD, 0xDD, 0xDD]` | Zigbee extended pan ID.                                                                                                             |
| channel      | integer  | `11`                                               | ZigBee channel (Note: Use a ZLL channel: 11, 15, 20, or 25 to avoid Problems).<br>Note: Changing requires repairing of all devices. |
| network_key  | string   | `'!network_key network_key'`                       | Network encryption key, will improve security.<br>Note: Changing requires repairing of all devices.                                 |

### zigbee_serial

Details of all settings can be found in the [configuration guide for adapter settings](https://www.zigbee2mqtt.io/guide/configuration/adapter-settings.html).

| Variable | Type     | Default        | Comments                                          |
|----------|----------|----------------|---------------------------------------------------|
| port     | string   | `/dev/ttyACM0` | Location of the zigbee USB adapter.               |
| baudrate | integer  | `115200`       | Baud rate speed for serial port.                  |
| rtscts   | boolean  | `false`        | RTS / CTS Hardware Flow Control for serial port.  |

### zigbee_frontend

Details of all frontend settings cen be found in the [configuration guide for fronted](https://www.zigbee2mqtt.io/guide/configuration/frontend.html).

| Variable | Type    | Default   | Comments                                         |
|----------|---------|-----------|--------------------------------------------------|
| port     | integer | `8080`    | Port the frontend is listening to.               |

### zigbee_ota

Details of all settings can be found in the [configuration guide for OTA device firmware update](https://www.zigbee2mqtt.io/guide/configuration/ota-device-updates.html#ota-index-override-file).

| Variable                        | Type     | Default | Comments                                                         |
|---------------------------------|----------|---------|------------------------------------------------------------------|
| ikea_ota_use_test_url           | boolean  | `false` | Use IKEA TRADFRI OTA test server, see OTA updates documentation. |
| update_check_interval           | integer  | `1440`  | Minimum time between OTA update checks.                          |
| disable_automatic_update_check  | boolean  | `false` | Disable automatic update checks.                                 |

## Dependencies

- git
- npm >=5.8
- nodejs >=10

## Example Playbook

To install zigbee2mqtt with default serial port:

```yaml

    - name: zigbee2mqtt octoprint on raspbian
      hosts: ip_address_of_rpi
      become: true

      roles:
      - finalgene.zigbee2mqtt
```

To install zigbee2mqtt with custom serial port:

```yaml

    - name: zigbee2mqtt octoprint on raspbian
      hosts: ip_address_of_rpi
      become: true

      roles:
      - role: finalgene.zigbee2mqtt
        vars: 
          zigbee_serial:
            port: /dev/serial/by-id/usb-Texas_Instruments_TI_CC2531_USB_CDC___0X00124B0018ED3DDF-if00
```

To install zigbee2mqtt with default serial port and MQTT authentication:

```yaml

    - name: zigbee2mqtt octoprint on raspbian
      hosts: ip_address_of_rpi
      become: true

      roles:
      - role: finalgene.zigbee2mqtt
        vars:
          zigbee_mqtt:
            user: mqtt_user
            password: mqtt_password
```

## License

BSD

## Author Information

- [igami](https://github.com/Igami)
- [Frank Giesecke](https://github.com/frankgiesecke)
