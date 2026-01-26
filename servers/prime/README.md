# Server prime
This is the 2026 server, build around a CWWK motherboard running [Ubuntu Server](https://ubuntu.com/server).

- *Motherboard:* CWWK [CW-AT-10G-8P](./docs/CW-AT-10G-8P-V1.0.pdf) with a [Intel N350](./docs/Intel_N350_Speccifications.csv).
- *Memory:* Samsung 32GB DDR5-5600 SO-DIMM ([M425R4GA3EB0-CWM](https://semiconductor.samsung.com/dram/module/sodimm/m425r4ga3eb0-cwm/))
- *Storage:* [Vi5000 PCIe NVMe™ M.2 SSD](https://www.verbatim-europe.com/nl/internal-ssd/products/vi5000-pcie-nvme-m2-ssd-2tb-31827)

## Docker
For most services, I use Docker containers with named volumes for the persistant storage. These are all seperate ZFS datasets, created with specific optimizations for the intented use, i.e. database, small file storage, logs or caching. I've vibe-coded a script for ease of use. For that see the [zfs-container](https://github.com/rowdynl/linux-scripts/blob/main/bash/zfs-container.sh) bash script in my [linux-scripts](https://github.com/rowdynl/linux-scripts) repository.

### Docker networks
We have three docker networks:
- **dirty-water** - extra network to let some of the services in the new-providence-stack talk to each other without giving them access to databases or expose them to traefik.
- **web** - everything that needs to be access from the big, scary internet uses this network. A reverse proxy connects to this network and routed traffic to the containers.
- **backend** - everything that does not need to communicate with the outside world. In here you's find containers like databases and such.
- **mcvlan** - everything that needs to have an IP on the local network, see below.

#### Docker network macvlan
Some containers like Plex or Home-Assistant need to be 'on' the local network to work properly, i.e. for DLNA discovery, mDNS, SSDP. Instead of using network_mode 'host', we use a special network where we will give them a fixed IP. How that's configured in the docker compose files, see the compose files in the different stacks, i.e. the plex service in the [new-providence](https://github.com/rowdynl/komodo/blob/main/servers/prime/stacks/new-providence-island/compose.yaml) stack. 

This command creates 8 usable IPs (200–206):

```
docker network create -d macvlan \
  --subnet=192.168.178.0/24 \
  --gateway=192.168.178.1 \
  --ip-range=192.168.178.200/29 \
  -o parent=enp1s0 \
  macvlan
``` 

To make sure the containers can also access the host; the host also need to have a IP in that macvlan network:

```
ip link add macvlan0 link enp1s0 type macvlan mode bridge
ip addr add 192.168.178.254/32 dev macvlan0
ip link set macvlan0 up
```

**NOTE:** *This is temporary and need to be performed after each reboot. The persistant setup must be done, however, last time it trashed my network, so that's a future work item*