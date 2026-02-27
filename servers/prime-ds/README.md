# Server prime
This is my old server, a ASRock J4105 motherboard running [XPenology(https://xpenology.tech/)].

- *Motherboard:* ASRock [J4105-ITX](https://www.asrock.com/mb/Intel/J4105-ITX/) with a [Intel Celeron J3455](./docs/Intel_J3455_Specifications.csv) processor.
- *Memory:* Samsung 16GB DDR4-2133 SO-DIMM ([M471A1G43DB0-CPB](https://semiconductor.samsung.com/dram/module/ecc-sodimm/m474a1g43db0-cpb/))
- *Storage:*
    - 1x Western Digital 3TB Green (WD30EZRX-00SPEB0)
    - 1x Western Digital 3TB Green (WD30EFRX-68EUZN0)
    - 2x Western Digital 6TB Red Plus NAS ([WD60EFZX-68B3FN0](https://www.westerndigital.com/nl-nl/products/internal-drives/wd-red-plus-sata-3-5-hdd?sku=WD60EFPX))

## Docker
For most services, I use Docker containers a mapping to the local filesystem for persistent storage. Containers are mostly managed via [Komodo](https://komo.do/) running on my new server, [stack komodo](../prime/stacks/komodo/) (what's in a name).
