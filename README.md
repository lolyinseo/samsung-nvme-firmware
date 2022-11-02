# SAMSUNG nvme firmware
Collection of SAMSUNG NVMe SSD firmware

## Firmware upgrade
Almost all modern SSD drives support firmware upgrade via NVM Express commands. For linux you can use https://github.com/linux-nvme/nvme-cli

List all the NVMe SSDs

    $ sudo nvme list
    
    Node             SN                   Model                                    Namespace Usage                      Format           FW Rev  
    ---------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
    /dev/nvme0n1     S27FNY0HB04880       SAMSUNG MZVPV512HDGL-000H1               1         163.17  GB / 512.11  GB    512   B +  0 B   BXW74H0Q

## Model decoding

| M | Z | X   | X  | X  | X  | X  | X  |X   | X  | X  |  X | -  |  X |  X | X  | X  | X  |
| ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
|  1 |  2 |3   |  4 |  5 |  6 | 7  |  8 |  9 |10   |11   |12   | 13  |14   |15   | 16  | 17  | 18  | 

|##### | Type   | Value|
| :------------ | :------------ | :------------ |
| 1  | Type  |Memory (M)  |
|  2 |Module Classification   | Z: SSD|
|   3| Form Factor  |V: PCIeM.2 (22*80, PCIe x4), Q: PCIe2.5 inch 7mmt, 7: 2.5" 7mmT SATA |
|   4| Line-Up  |L: Client/SV (VNAND 3bit MLC), K: Client/SV (VNAND 2bit MLC) |
|   5| Controller  |B: Phoenix, 2: Elpis, T: RFX, W: Polaris |
|  6-8| SSD Density  | 256, 512, 1T0, 2T0 |
|   9| NAND PKG & NAND Voltage  | H: BGA (LF,HF) |
|   10| Flash Generation  | M: 1st Generation, A: 2nd, B: 3rd, C: 4th, D: 5th, E: 6th ... |
|   11-12| NAND Density  |JR: 2T ODP 2CE (FBI), LS: 4T HDP 2CE(FBI), LA: 8T HDP 2CE(FBI), HQ: 1T QDP 4CE, JQ: 2T ODP 4CE, LR: 4T HDP 4CE, LB: 8T HDP 4CE |
|   14|  Default |0 |
|   15| HW revision  |0: No revision |
|   16|Packaging type   | 0: Bulk, A: General|
|   17-18| Customer  |00: World wide (non-SED), 07: World wide (SED), H7 - HP SED, L7 - Lenovo SED, D7 - Dell SED, etc |


