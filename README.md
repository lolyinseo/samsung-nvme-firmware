# SAMSUNG nvme firmware
Collection of SAMSUNG NVMe SSD firmware

## Firmware upgrade
Almost all modern SSD drives support firmware upgrade via NVM Express commands. For linux you can use https://github.com/linux-nvme/nvme-cli

List all the NVMe SSDs

    $ sudo nvme list
    
    Node             SN                   Model                                    Namespace Usage                      Format           FW Rev  
    ---------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
    /dev/nvme0n1     S27FNY0HB04880       SAMSUNG MZVPV512HDGL-000H1               1         163.17  GB / 512.11  GB    512   B +  0 B   BXW74H0Q

Get some info

    $ sudo nvme id-ctrl -H /dev/nvme0
    
    NVME Identify Controller:
    vid     : 0x144d
    ssvid   : 0x144d
    sn      : S27FNY0HB04880
    mn      : SAMSUNG MZVPV512HDGL-000H1
    fr      : BXW74H0Q
    rab     : 2
    ieee    : 002538
    cmic    : 0
      [2:2] : 0 PCI
      [1:1] : 0 Single Controller
      [0:0] : 0 Single Port
    mdts    : 5
    cntlid  : 1
    ver     : 0
    rtd3r   : 0
    rtd3e   : 0
    oaes    : 0
      [8:8] : 0 Namespace Attribute Changed Event Not Supported
    oacs    : 0x7
      [3:3] : 0 NS Management and Attachment Not Supported
      [2:2] : 0x1 FW Commit and Download Supported
      [1:1] : 0x1 Format NVM Supported
      [0:0] : 0x1 Sec. Send and Receive Supported
    acl     : 7
    aerl    : 3
    frmw    : 0x6
      [4:4] : 0 Firmware Activate Without Reset Not Supported
      [3:1] : 0x3 Number of Firmware Slots
      [0:0] : 0 Firmware Slot 1 Read/Write
    lpa     : 0x1
      [1:1] : 0 Command Effects Log Page Not Supported
      [0:0] : 0x1 SMART/Health Log Page per NS Supported
    elpe    : 63
    npss    : 4
    avscc   : 0x1
      [0:0] : 0x1 Admin Vendor Specific Commands uses NVMe Format
    apsta   : 0x1
      [0:0] : 0x1 Autonomous Power State Transitions Supported
    wctemp  : 0
    cctemp  : 0
    mtfa    : 0
    hmpre   : 0
    hmmin   : 0
    tnvmcap : 0
    unvmcap : 0
    rpmbs   : 0
     [31:24]: 0 Access Size
     [23:16]: 0 Total Size
      [5:3] : 0 Authentication Method
      [2:0] : 0 Number of RPMB Units
    sqes    : 0x66
      [7:4] : 0x6 Max SQ Entry Size (64)
      [3:0] : 0x6 Min SQ Entry Size (64)
    cqes    : 0x44
      [7:4] : 0x4 Max CQ Entry Size (16)
      [3:0] : 0x4 Min CQ Entry Size (16)
    nn      : 1
    oncs    : 0x1f
      [5:5] : 0 Reservations Not Supported
      [4:4] : 0x1 Save and Select Supported
      [3:3] : 0x1 Write Zeroes Supported
      [2:2] : 0x1 Data Set Management Supported
      [1:1] : 0x1 Write Uncorrectable Supported
      [0:0] : 0x1 Compare Supported
    fuses   : 0
      [0:0] : 0 Fused Compare and Write Not Supported
    fna     : 0
      [2:2] : 0 Crypto Erase Not Supported as part of Secure Erase
      [1:1] : 0 Crypto Erase Applies to Single Namespace(s)
      [0:0] : 0 Format Applies to Single Namespace(s)
    vwc     : 0x1
      [0:0] : 0x1 Volatile Write Cache Present
    awun    : 255
    awupf   : 0
    nvscc   : 1
      [0:0] : 0x1 NVM Vendor Specific Commands uses NVMe Format
    acwu    : 0
    sgls    : 0
      [0:0] : 0 Scatter-Gather Lists Not Supported
    ps    0 : mp:9.00W operational enlat:5 exlat:5 rrt:0 rrl:0
              rwt:0 rwl:0 idle_power:- active_power:-
    ps    1 : mp:4.00W operational enlat:30 exlat:30 rrt:1 rrl:1
              rwt:1 rwl:1 idle_power:- active_power:-
    ps    2 : mp:3.00W operational enlat:100 exlat:100 rrt:2 rrl:2
              rwt:2 rwl:2 idle_power:- active_power:-
    ps    3 : mp:0.0700W non-operational enlat:500 exlat:5000 rrt:3 rrl:3
              rwt:3 rwl:3 idle_power:- active_power:-
    ps    4 : mp:0.0050W non-operational enlat:2000 exlat:22000 rrt:4 rrl:4
              rwt:4 rwl:4 idle_power:- active_power:-

As we see the disk support Firmware Image Download command (0x1 FW Commit and Download Supported), has 3 Firmware Slots (0x3 Number of Firmware Slots) and 1st Firmware Slot in Read/Write mode(0 Firmware Slot 1 Read/Write) but I don't recommend using it.. Try to use free Firmware Slot, do not overwrite the original firmware.

Lets download firmware to controller SRAM

    $ sudo nvme fw-download -f firmware.bin /dev/nvme0
    
    Firmware download success

Write firmware to free Firmware Slot 2 without activation

    $ sudo nvme fw-commit -s 2 -a 0 /dev/nvme0
    
    Success committing firmware action:0 slot:2
    
Activate new firmware
    
     $ sudo nvme fw-commit -s 2 -a 2 /dev/nvme0
    
    Success committing firmware action:2 slot:2
    
Reboot and pray )    

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
|   5| Controller  |B: Phoenix, 2: Elpis, 7: Pascal, T: RFX, W: Polaris, 4: MARVELL |
|  6-8| SSD Density  | 256, 512, 1T0, 2T0 |
|   9| NAND PKG & NAND Voltage  | H: BGA (LF,HF) |
|   10| Flash Generation  | M: 1st Generation, A: 2nd, B: 3rd, C: 4th, D: 5th, E: 6th ... |
|   11-12| NAND Density  |JR: 2T ODP 2CE (FBI), LS: 4T HDP 2CE(FBI), LA: 8T HDP 2CE(FBI), HQ: 1T QDP 4CE, JQ: 2T ODP 4CE, LR: 4T HDP 4CE, LB: 8T HDP 4CE |
|   14|  Default |0 |
|   15| HW revision  |0: No revision |
|   16|Packaging type   | 0: Bulk, A: General|
|   17-18| Customer  |00: World wide (non-SED), 07: World wide (SED), H7 - HP SED, L7 - Lenovo SED, D7 - Dell SED, etc |


