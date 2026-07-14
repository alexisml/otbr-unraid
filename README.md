# OpenThread Border Router for Unraid

Unraid Docker template for running an OpenThread Border Router (OTBR) using the [hass-otbr-docker](https://github.com/ownbee/hass-otbr-docker) image.

## Modes

- **LAN / network-based RCP (default):** set `NETWORK_DEVICE` to your RCP endpoint (e.g. `10.10.3.18:6638`) and keep `DEVICE` as `/tmp/ttyOTBR`.
- **USB radio:** clear `NETWORK_DEVICE`, map your USB device in `USB Radio Device`, and set `DEVICE` to the internal mapped path (e.g. `/dev/ttyUSB0`).

## Prerequisites

This template uses `--net=host` and `--privileged=true`, so kernel sysctls must be applied on the Unraid host itself. Add a User Script that runs at array startup:

```bash
#!/bin/bash
sysctl -w net.ipv6.conf.all.disable_ipv6=0
sysctl -w net.ipv6.conf.all.forwarding=1
sysctl -w net.ipv6.conf.br0.accept_ra=2
sysctl -w net.ipv6.conf.eth0.accept_ra=2
sysctl -w net.ipv4.conf.all.forwarding=1
```

Adjust interface names (`br0`, `eth0`) to match your host.

## Tested setup

- **Hardware:** SMLIGHT MR4U with EFR32MG26 set as Thread to remote OTBR
- **Connection:** LAN (IP:port)
- **USB passthrough:** not tested

## References

- Unraid Community Apps entry: [OpenThreadBorderRouter-HA](https://ca.unraid.net/apps/openthreadborderrouter-ha-0hkzjrk0hn1sy4)
- Original template: [ds-sebastian/unraid-templates](https://raw.githubusercontent.com/ds-sebastian/unraid-templates/main/OpenThreadBorderRouter-HA/OpenThreadBorderRouter-HA.xml)
- Docker image: [ownbee/hass-otbr-docker](https://github.com/ownbee/hass-otbr-docker)
