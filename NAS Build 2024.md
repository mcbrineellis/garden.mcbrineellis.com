My current home lab and storage setup:

**Compute Nodes (ESXi/Proxmox nodes)**
- Dell USFF, HP USFF x2
  - 2 TB SATA SSD internal storage on each node

**External Storage (SSD/ HDD)**
- OWC 2 disk Thunderbolt 3 dock, connects to my MacBook Air
	- Noisy fans
	- I have to physically be at my desk to use it
	- The speed 
### ECC vs Non-ECC RAM
If using ZFS, important because there is so much caching done in memory (data can potentially be held in RAM for a long period of time, increasing the possibility of bit flips). If using something like Unraid then less of a priority.

However... ECC support is a bit of a minefield.  This "Wolfgang's Channel" is a good explainer: https://www.youtube.com/watch?v=J_WJI4hp_B8

In short, *Intel CPUs* are hit or miss on ECC because Intel treats it as a special feature that they only enable on certain CPUs.  *AMD CPUs* all support ECC, but *AMD APUs* (with integrated graphics) generally don't unless they're from the "PRO" series but those are more expensive and hard to obtain unless through grey market because they don't sell them to consumers.

So in the end, you are forced to only prioritize 3 out of these 4 "features":
- Power efficiency
- Availability
- ECC Support
- QuickSync
### Intel vs AMD
**Intel Pros**
- Intel idle power usage is much lower than AMD.
- Intel QuickSync is good for transcoding media (Plex?)
**Intel Cons**
- 
**AMD Pros**
- AMD supports ECC memory more widely
**AMD Cons**
- AMD APUs (with integrated graphics) don't support ECC memory

AMD Ryzen 7 PRO 4750G are available on eBay for around ~$150
# NAS Style Micro ATX Case (Node 804)
This case is larger than the Jonsbo N3, but much easier to source parts for because it supports larger motherboards, and standard ATX PSUs.

Corsair RM750X PSU.
# NAS Style Mini ITX Case (Jonsbo N3)
**Jonsbo N3** is my favourite so far of the "NAS-Style" cases where the drives are accessed from the front.  It has a low profile design and 8 drive slots.  

Quite pricey but... other cases with external 3.5" bays are pricey too...
- AUDHEID 8-Bay NAS Mini ITX Chassis (2023 New) $350 https://www.amazon.ca/dp/B0BXD8QSQK?th=1&ref_=as_li_ss_tl&language=en_US&linkCode=gs2&linkId=4f5eead3b950e84faa00ca586c656d46&tag=craftcomput06-20
- SilverStone CS382 $250
### Pros
- 8 bays for 3.5" SATA drives are accessible super easily
- Really small footprint, looks cool
### Cons
- Super expensive, compared to larger cases...
	- $275 on AliExpress (shipping cost is half of that...)
	- $280 from NewEgg (free shipping)
- Mini-ITX motherboards usually only have 4x SATA, so to use all 8 drives
	- Install a PCI HBA adapter
	- Use an M.2 slot
	- More on this later...
### Example builds I found
- https://ca.pcpartpicker.com/b/JsfPxr
- Build that I got the idea for the PCIe splitter from
	- Update 1: https://www.reddit.com/r/sffpc/comments/197cg0i/jonsbo_n3_server_4x_m2_nvme10x_sata_gpu_lp/
	- Update 2: https://www.reddit.com/r/sffpc/comments/1ataflg/jonsbo_n3_server_with_space_for_13_drives/
- https://www.reddit.com/r/homelab/comments/16k3gzf/datacenter_in_a_box_utimate_home_server/
- https://forums.unraid.net/topic/145100-build-report-jonsbo-n3-cwwk-j6413-soc/
### Quirks to note

**It has some sort of extra SATA power port:**
https://smallformfactor.net/forum/threads/jonsbo-n3-itx-nas-case-backplane.18942/
> I see no reason for the extra _sata_ power _port_ since it's only rated for 54w it just seems redundant to me.

**The rear 100mm stock fans can be replaced with 92mm Noctua fans**
https://forums.overclockers.co.uk/threads/jonsbo-n3.18976199/post-36662426
> The choice of dual 3-pin 100m fans for the rear is a very strange decision.

https://forums.overclockers.co.uk/threads/jonsbo-n3.18976199/post-36687516
> Noctua nh a9 92mm fans fit perfectly in place of the 100mm jonsbo ones.

### Working around stuff

Using the M.2 and PCIe slots to the max.

- **PCIe Bifurcation (splitter card) PH43** - splitter divides the PCIe x16 slot into two m.2 nvme x4 on both sides of the card and an x16 slot with x8 speed on the top
	- Allows for use of PCIe for GPU or 10Gbe NIC
	- 2x M.2 slots can be used for SSD, or M.2 to SATA adapter such as ASM1166, JMB585.
- Can replace built-in Wi-FI with additional 2.5G NIC
	- https://www.aliexpress.com/item/1005005101361503.html
	- 2.5G Ethernet LAN Card RTL8125B that connects via M.2
- You can bring out 4 pcie lanes from an unused m.2 slot on the mainboard via an adapter cable (just google m.2 to pcie cable) and use that for a 10gbit ethernet card
	- This *M2 TO PCIE Riser PCIe x4 PCI-E3.0 4x To M.2 NVMe M Key 2280 Riser Card Gen3.0 Cable M2 Key-M PCI-Express Extension cord 32G/bps* extension cable allows you to connect an X4 card to an M.2 slot. https://www.aliexpress.com/item/1005003136139461.html for ~$25 CAD
	- I found a *Mellanox MCX311A-XCAT ConnectX-3 EN 10G Ethernet 10GbE SFP+ PCIe NIC LC Cable* on eBay, comes with LC cables and 2 SFP+ transceivers for ~$41.23 CAD
	- On my Mac, could then get a QNAP Thunderbolt 3 to 10GbE Adapter for $285 CAD

Cool tree I built with ChatGPT showing how everything in that build linked above is connected:

```
PC Build for Unraid NAS in Jonsbo N3 Case with PCIe Splitter and Cache SSDs
├── AM4 CPU
│   ├── PCI Express 4.0/3.0 Bus
│   │   └── PCIe x16 to x8+x4+x4 Splitter Adapter Card (PH43)
│   │       ├── x8 Connection
│   │       │   └── GPU: Asrock Intel Arc A380 LP or RTX 4060 (using only x8 lanes)
│   │       ├── x4 Connection [1] (Front .2 NVME Slot on Adapter)
│   │       │   └── M.2 NVMe SSD (Cache SSD for Unraid NAS)
│   │       └── x4 Connection [2] (Back .2 NVME Slot on Adapter)
│   │           └── M.2 to 6 SATA Adapter
│   │               └── 6 SATA Ports (for additional SATA devices)
│   ├── M.2 Socket 3 (M2A_CPU)
│   │   └── M.2 NVMe SSD (Primary Cache SSD for Unraid NAS)
│   └── Connection to AMD B550 via PCIe 3.0 x4
└── AMD B550 Chipset
    ├── 4 SATA 6Gb/s Ports (for SATA devices like HDDs or SSDs)
    ├── M.2 WIFI Module Slot
    │   └── [Default] WIFI Module
    │   └── [Alternate] 2.5G Ethernet LAN Card (RTL8125B, via M.2 A+E)
    └── M.2 Socket 3 (M2B_SB) - Unused in this configuration
```

Ope, the same guy just posted an update:
https://www.reddit.com/r/sffpc/comments/1ataflg/jonsbo_n3_server_with_space_for_13_drives/
### Parts list
[[NAS Build 2024 Part Arrival Tracker]]
- **Motherboard**
	- Brandon's motherboard, **Gigabyte B550I Aorus Pro AX** https://www.reddit.com/r/Amd/comments/ny4ch2/ecc_on_5800x_gigabyte_b550i/ $229.12 (185.76 + 17.00 shipping + 26.36 tax) - eBay
		- Supports ECC!
- *CPU*
	- AMD Ryzen 5 Pro 5650G 3.9 Ghz https://postnl.post/#/tracking/items/LR044950525NL
- *RAM*
	- *ECC?* 2x32g? 2x32g?
- *Storage*
	*- USB stick for unraid - have one in my Amazon cart
	- Good m.2 SSDs for cache - need to research*
	- ASM1166 M.2 NVME to SATA 3.0 $24.87 - https://www.aliexpress.com/item/1005006394563922.html
*- PSU - SFX*
*- Fans
- More Noctua fans?
- GPU*
- **CPU Cooler**
	- Noctua NH-U9S $72.95 (45.00 + 19.56 shipping + 8.39 tax) - Amazon Canada
- **Adapters**
	- PCIe x16 to x8+x4+x4 Splitter Adapter Card (PH43) C$21.41 - https://www.aliexpress.com/item/1005005277952427.html
- **Drives**
	- RECERTIFIED Seagate Ironwolf Pro 22TB SATA 3.5" Enterprise NAS ST22000NT001C $1,349.19 (1,142.97 + 51.00 shipping + 155.22 tax) - eBay
		- These are supposed to be quiet.
		- A user on [Reddit](https://www.reddit.com/r/DataHoarder/comments/10866m0/comment/j3yuhx0/?utm_source=reddit&utm_medium=web2x&context=3) linked to some cool German website which tested a bunch of high capacity HDDs: https://www.hardwareluxx.de/index.php/artikel/hardware/storage/57753-seagate-ironwolf-pro-im-test-20-tb-durch-10-platter.html?start=3
		- They tested the 20TB, the serial of the 22TB is so similar though so I am assuming it will have similar characteristics... we'll see though

### Parts I didn't get but considered
- To get a true AM4 server motherboard with IPMI for remote control, 2x 10Gbit Intel NIC, 8x SATA and stuff... X570D4I-2T.  $500+.  And requires specific RAM (ECC SO-DIMM is rare).  Probably out of the budget but pretty cool.
		- https://www.servethehome.com/asrock-rack-x570d4i-2t-amd-ryzen-server-in-mitx/
		- https://www.reddit.com/r/sffpc/comments/lymbka/asrock_rack_x570d4i2t_can_easily_be_made/
# Avoiding corruption with Unraid (bit rot and such...)
Absolute power corrupts absolutely?

https://forums.unraid.net/topic/124149-parity-check-corruption-parity-disk-or-data-disk/?do=findComment&comment=1132100
> The only way to know where the corruption occurred is if you are using btrfs file system on your array disks (in which case a scrub will tell you about corrupt files) or if you have checksums for your files when running xfs/reiserfs.

https://forums.unraid.net/topic/80830-dealing-with-silent-corruption-aka-bitrot/?do=findComment&comment=751630
> If you are truly concerned about data corruption you should have a MB with ECC memory installed.  Many of us feel that most 'Bit-rot' problems are actually memory issues (ex., bit-flipping due cosmic radiation, static discharge, or power surges) as modern hard drives are designed to have detect and correct any disk errors as they occur on-the-fly.  If the HD can not correct the issue, it reports a read error.)

So basically:
- **Use good hardware**... use ECC memory if possible.
- **Implement a good backup strategy**... 3-2-1 backup, with enough retention and perhaps WORM storage for protection against ransomware.
- **Use checksums**... so you are aware that corruption / bit rot has occurred and can restore a backup .
	- Checksums will ensure that files haven't corrupted after they were originally written.
		- Of course, if the file was corrupted on the initial copy then you are out of luck.
	- If a file becomes corrupted and you're unaware of it, then when you finally go to restore it, your backup may also be corrupted (maybe it corrupted at a time *before* your backup history / retention period)
## So which FS should I pick?
### ZFS ("pure z-pool")
Since we want to use the Unraid parity-protected array, ZFS is not an option.

It has advantages, such as snapshots, compression and self-healing of bitrot.  but, I think Btrfs can cover us for those use-cases while still allowing us to use Unraid's parity.

Plus, ZFS has drawbacks.  One being, like any other RAID array, if you lose more disks than parity, you actually lose ALL your data, as a consequence of it being striped across all disks in the pool.

Unraid's parity manages to keep each disk as a separate filesystem, not just some scrambled data split across the whole pool.  Meaning even if all of your disks fail but one, that one disk is still readable and you can recover the data off of it.  Pretty dope!

https://forums.unraid.net/topic/80830-dealing-with-silent-corruption-aka-bitrot/?do=findComment&comment=751557
> One of the greatest strength of unRAID (in my opinion) is the fact that each disk is a separate file system.  So, as I'm sure you've heard before, if more disks fail than you have parity drives, you can still recover data from the remaining disks since they're each an independent file system.  Unlike in a RAID5 array, for example, which stripes data across multiple disks.  Lose too many, and _all_ your data is toast.

Uses too much RAM.
All disks spin all the time (more noise, more power usage).
### Btrfs
Newer fs, "less stable"
- Data integrity - I at least know when a file is bad, even though I may not be able to fix it/will have to get a copy.
	- Can schedule "scrubbing" to check all the files
- Snapshots - Something to aid in protecting me from user error once set up, something I feel is a bit lacking in general.
	- Can schedule snapshots to allow you to easily revert stuff

> I chose single drive BTRFS because I wanted to validate the integrity of my data (scrubs). The Unraid file integrity plugin is not 100% accurate.
> 
> I've read XFS is easier to repair, but I'm not sure in which scenario that applies to. I don't use BTRFS in a RAID configuration. I'm not affected by power outages. I do not have frequent write changes. Therefore I don't see any risks using BTRFS, plus I get a robust and portable data integrity feature (based on filesystem, not a plugin).

> I run regular unraid scrubs = detects ~~(and if needed repairs)~~ bitrot. If I ever get into the situation that I have unrecoverable bitrot (highly unlikely as I did not have ANY bitrot so far, so to progress to a stage where it is unrecoverable even with parity drive is a stretch of imagination), I have offsite-backups, one with a long-term rotation, so I could restore a known good state state before the bitrot.
> 
> Mount single drive: That's the best thing about unraid for me. The drives do not need to be btrfs-level or driver-level RAIDed. The fs on the drive is a normal single-drive btrfs, and unraid takes care of the parity layer independently of btrfs. So I have my content split on several drives which would look like normal single drives with a couple of files if mounted somewhere else, but to unraid it's a combined array with parity on top.
> 
> When I looked into it some time ago, btrfs raid 5 was still experimental and I did not fully trust it, plus, you cannot simply mount a single raid 5 btrfs drive and access/recover the contents, it would be useless due to the striping.
> 
> With unraid I got best of both worlds, single-disk mounts for recovery, striping, and btrfs snapshots.
### XFS
Default filesystem for Unraid.

There is a plugin that handles file integrity. Forum thread with info:
[https://forums.unraid.net/topic/43290-dynamix-file-integrity-plugin/](https://forums.unraid.net/topic/43290-dynamix-file-integrity-plugin/)

https://www.reddit.com/r/unRAID/comments/g12dfg/comment/fndpoww/?utm_source=reddit&utm_medium=web2x&context=3
> I've been running this for years. It's an interesting plugin but generates a lot of false positives if you are writing a massive number of files at once or through some files that are modified in place (ex docker.img)
> 
> I haven't tried to correct or decided which copy of a file is correct but the process would involve comparing the checksum values generated for each of the files.
>
> If bitrot correction is important to you unraid probably isn't the answer. I'd go with something like ZFS.
>
> For offsite backup you may also want to consider [Borg](https://www.borgbackup.org/) or [restic](https://restic.net/). They maintain versioned, de duplicated backups of your data. **If bitrot on your main copy does occur it will be detected as a "change" during backup.**  You can catch that in the logs and decide for yourself whether the file should have changed or whether it was a case of bitrot.  Either way you have a versioned copy of the deal and you can decide to recover whichever one you want.
> 
> As a side note, you might be interested in [QCTools](https://bavc.github.io/qctools/) which is designed to analyze videos. One of the key use cases is validation of video archive integrity.


