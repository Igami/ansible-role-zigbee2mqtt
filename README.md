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

| Variable                        | Type         | Default                                     | Comments                                                                                                                                                          |
|---------------------------------|--------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| zigbee_user                     | string       | `zigbee`                                    |                                                                                                                                                                   |
| zigbee_user_groups              | string       | `tty,dialout`                               |                                                                                                                                                                   |
| zigbee_user_append              | boolean      | `false`                                     |                                                                                                                                                                   |
| zigbee_dir                      | string       | `/opt/zigbee2mqtt`                          |                                                                                                                                                                   |
| zigbee_repository               | string       | `https://github.com/Koenkk/zigbee2mqtt.git` |                                                                                                                                                                   |
| zigbee_homeassistant            | boolean      | `false`                                     |                                                                                                                                                                   |
| zigbee_permit_join              | boolean      | `true`                                      |                                                                                                                                                                   |
| zigbee_mqtt_base_topic          | string       | `zigbee2mqtt`                               |                                                                                                                                                                   |
| zigbee_mqtt_server              | string       | `mqtt://localhost`                          |                                                                                                                                                                   |
| zigbee_serial_port              | string       | `/dev/ttyACM0`                              |                                                                                                                                                                   |
| zigbee_mqtt_user                | string       |                                             |                                                                                                                                                                   |
| zigbee_mqtt_password            | string       |                                             |                                                                                                                                                                   |
| zigbee_network_key              | string/array | `'!network_key network_key'`                | Zigbee2mqtt uses a known default encryption key. Therefore it is recommended to use a different one. By default this role will create an random key at firstrun.  |
| zigbee_generate_new_network_key | boolean      | `false`                                     |                                                                                                                                                                   |

## Dependencies

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
          zigbee_serial_port: /dev/serial/by-id/usb-Texas_Instruments_TI_CC2531_USB_CDC___0X00124B0018ED3DDF-if00
```

To install zigbee2mqtt with default serial port and MQTT authentication:

```yaml

    - name: zigbee2mqtt octoprint on raspbian
      hosts: ip_address_of_rpi
      become: true

      roles:
      - role: finalgene.zigbee2mqtt
        vars: 
          zigbee_mqtt_user: mqtt_user
          zigbee_mqtt_password: mqtt_password
```

## License

BSD

## Author Information

- [igami](https://github.com/Igami)
- [Frank Giesecke](https://github.com/frankgiesecke)
