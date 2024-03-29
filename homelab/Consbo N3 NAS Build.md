Finally, I have decided that enough is enough and I am building an epic storage server to centralize all of my digital artifacts and creative detritus.

I used a case called Jonsbo N3, so I have called this build... the "Consbo N3".  Clever, right?
# State of the Homelab pre-NAS
My pre-existing home lab and storage setup looked like this:

- Everything Everywhere All at Once... and no idea where any thing ever is at any time :'(
- Nothing properly backed up
	- A lot of data just sitting on a single cloud storage & nowhere else

**Compute Nodes (ESXi/Proxmox nodes)**
- Dell USFF x1, HP USFF x2
  - 2 TB SATA SSD internal storage on each of these nodes
  - USB 2.5GbE Ethernet NICs and a 2.5GbE switch I got off of AliExpress for super cheap!

**External Storage (SSD/ HDD)**
- OWC 2 disk Thunderbolt 3 dock, connects to my MacBook Air
	- Noisy fans
	- I have to physically be at my desk to use it
	- My computer has to be powered on and plugged into it so it can back up
- 2x 8TB SMR Seagate external HDDs
- 3 portable 2TB SSDs
- 1 portable 5TB HDD
- A whole pile of old bulky external HDDs - this data I already moved to the Thunderbolt dock

**Cloud storage**
- Google Drive 2TB
- iCloud 2TB
- Adobe Lightroom 1TB
- Flickr plan
- Backblaze B2 object store bucket
- Amazon S3, Glacier object store buckets for some archival backup stuff

HDDs are cheap!  The only thing that held me back from building a NAS was:
- Moving a lot over the past year meant it would be a hassle to bring with me
- Upfront cost of building the NAS
- I thought I would figure out a way to set something up in the cloud to serve the function of a NAS, but this didn't pan out
	- Say it with me: "The cloud is just someone else's computer!"
# Case options
## NAS Style Micro ATX Case (Node 804)
This case is larger than the Jonsbo N3, but much easier to source parts for because it supports larger motherboards, and standard ATX PSUs.

Could use Corsair RM750X PSU.

I decided against this case because I really wanted hot swap bays, and the Jonsbo N3 looked cooler - and was quite a bit smaller too!  Plus I wanted the challenge of building with Mini-ITX.
## NAS Style Mini ITX Case (Jonsbo N3) - what I went with
**Jonsbo N3** is my favourite so far of the "NAS-Style" cases I've looked at where the drives are accessed from the front.  It has a low profile design and 8 drive slots.

On first glance, kinda pricey but... other cases with external 3.5" bays are pricey too...
- AUDHEID 8-Bay NAS Mini ITX Chassis (2023 New) $350 https://www.amazon.ca/dp/B0BXD8QSQK?th=1&ref_=as_li_ss_tl&language=en_US&linkCode=gs2&linkId=4f5eead3b950e84faa00ca586c656d46&tag=craftcomput06-20
- SilverStone CS382 $250
### N3 Pros
- 8 bays for 3.5" SATA drives are accessible super easily
- Really small footprint, looks cool
### N3 Cons
- Still expensive, compared to larger cases...
	- $275 on AliExpress (shipping cost is half of that...)
	- $280 from NewEgg (free shipping)
- Mini-ITX motherboards usually only have 4x SATA, so to use all 8 drives
	- Gotta install a PCI HBA adapter
	- Or, use an M.2 slot
		- More on this later...
### Jonsbo N3 Buildspiration
- https://ca.pcpartpicker.com/b/JsfPxr
- The build that I got the most inspo from!  Absolute beast of a system :)
	- Update 1: https://www.reddit.com/r/sffpc/comments/197cg0i/jonsbo_n3_server_4x_m2_nvme10x_sata_gpu_lp/
	- Update 2: https://www.reddit.com/r/sffpc/comments/1ataflg/jonsbo_n3_server_with_space_for_13_drives/
- https://www.reddit.com/r/homelab/comments/16k3gzf/datacenter_in_a_box_utimate_home_server/
- https://forums.unraid.net/topic/145100-build-report-jonsbo-n3-cwwk-j6413-soc/
### Quirks to note
**It has some sort of "extra" SATA power port on the backplane:**
https://smallformfactor.net/forum/threads/jonsbo-n3-itx-nas-case-backplane.18942/
> I see no reason for the extra _sata_ power _port_ since it's only rated for 54w it just seems redundant to me.

**What I did:** There is 2x Molex and 1x SATA plugs on the backplane for power.  So, I bought SATA to Molex adapters for the 2x SATA daisy cables that come from my PSU and connected those to the last plug on both cables, and one of the daisy-chain SATA connectors on the second cable also connects to the SATA plug.

- PSU SATA cable 1 -> 1x Molex
- PSU SATA cable 2 -> 1x Molex + 1x SATA power

**The rear 100mm stock fans can be replaced with 92mm Noctua fans**
https://forums.overclockers.co.uk/threads/jonsbo-n3.18976199/post-36662426
> The choice of dual 3-pin 100m fans for the rear is a very strange decision.
https://forums.overclockers.co.uk/threads/jonsbo-n3.18976199/post-36687516
> Noctua nh a9 92mm fans fit perfectly in place of the 100mm jonsbo ones.

**What I did:**  Replaced the rear fans with the 92mm Noctuas, and I didn't connect them to the 3-pin headers on the storage backplane, but instead used the extension and splitter cables that come with in the Noctua box and connected them to a fan header on my motherboard so I could control the fan speed.  Unfortunately, my motherboard (probably most are the same) doesn't have a way to set fan speed based on disk temps.  This isn't an issue in practice since the disk temps are fine.
### Using the M.2 and PCIe slots to the max
*Maximizing the potential of your Mini-ITX mobo :)*
- **PCIe Bifurcation (splitter card) PH43** - splitter divides the PCIe x16 slot into two m.2 nvme x4 on both sides of the card and an x16 slot with x8 speed on the top
	- Allows for use of PCIe for GPU or 10Gbe NIC
	- 2x M.2 slots can be used for 2x M.2 SSDs, or use of an M.2 to SATA adapter such as ASM1166, JMB585 to supplement any SATA ports already on the mobo.
- Can replace built-in Wi-FI with additional 2.5G NIC
	- https://www.aliexpress.com/item/1005005101361503.html
	- 2.5G Ethernet LAN Card RTL8125B that connects via M.2
- Apparently you can bring out 4 pcie lanes from an unused m.2 slot on the mainboard via an adapter cable (just google m.2 to pcie cable) and use that for a 10gbit ethernet card!
	- This *M2 TO PCIE Riser PCIe x4 PCI-E3.0 4x To M.2 NVMe M Key 2280 Riser Card Gen3.0 Cable M2 Key-M PCI-Express Extension cord 32G/bps* extension cable allows you to connect an X4 card to an M.2 slot. https://www.aliexpress.com/item/1005003136139461.html for ~$25 CAD
	- I found a *Mellanox MCX311A-XCAT ConnectX-3 EN 10G Ethernet 10GbE SFP+ PCIe NIC LC Cable* on eBay, comes with LC cables and 2 SFP+ transceivers for ~$41.23 CAD
	- On my Mac, could then get a QNAP Thunderbolt 3 to 10GbE Adapter for $285 CAD
	- As an addendum... I think 2.5GbE will be totally sufficient for my needs so I probably will never actually spend the $ to upgrade this system to 10G Ethernet.

Cool tree I built using ChatGPT to visualize how everything in that build linked above could be connected up:

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
# AMD or Intel?  ECC or no?
If using ZFS, important because there is so much caching done in memory (data can potentially be held in RAM for a long period of time, increasing the possibility of bit flips). If using something like Unraid then less of a priority.

However... ECC support is a bit of a minefield.  This "Wolfgang's Channel" is a good explainer: https://www.youtube.com/watch?v=J_WJI4hp_B8

In short, *Intel CPUs* are hit or miss on ECC because Intel treats it as a special feature that they only enable on certain CPUs.  *AMD CPUs* all support ECC, but *AMD APUs* (with integrated graphics) generally don't unless they're from the "PRO" series but those are more expensive and hard to obtain unless through grey market because they don't sell them to consumers.

Wolfgang explains in his video that you are sort of forced to choose 3 out of these 4 "features":
- Power efficiency
- Availability
- ECC Support
- QuickSync

**Intel Pros**
- Intel idle power usage is generally lower than AMD?
- Intel QuickSync is good for transcoding media (Plex?)
**Intel Cons**
- Hit or miss ECC support

**AMD Pros**
- AMD supports ECC memory more widely
- AMD is also supposed to be decently efficient... so which is better?  depends most on your setup I guess...
**AMD Cons**
- AMD APUs (with integrated graphics) don't support ECC memory unless you get PRO series
- If you need integrated graphics, spending extra $ to get the more rare PRO series
- No QuickSync

I looked into Mini-ITX AM4 server motherboards... which actually exist.  An AM4 server mobo with full IPMI for remote access, 2x 10Gbit Intel NIC, 8x SATA and stuff... the X570D4I-2T.

It's crazy pricy at $500+.  It also requires specific RAM (ECC SO-DIMM is rare).  So, out of the budget but still pretty cool:
- https://www.servethehome.com/asrock-rack-x570d4i-2t-amd-ryzen-server-in-mitx/
- https://www.reddit.com/r/sffpc/comments/lymbka/asrock_rack_x570d4i2t_can_easily_be_made/

So, I basically decided I would get the Gigabyte B550I AM4 motherboard (4x SATA!) to use the same motherboard my friend has (and will be repurposing for a NAS).
# Parts list
Prices are including tax and shipping, currency is CAD.

| Type                                  | Item                                                                                                                                                                                                                                                  | Price         |
| :------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------ |
| **Case**                              | Jonsbo N3 Mini ITX Desktop Case - *Newegg Canada*                                                                                                                                                                                                     | $280.24       |
| **Power Supply**                      | Corsair SF450 450 W 80+ Gold Certified Fully Modular SFX Power Supply - *eBay*                                                                                                                                                                        | $112.85       |
| **Motherboard**                       | Gigabyte B550I AORUS PRO AX Mini ITX AM4 Motherboard - *eBay*<br><br>Same motherboard my friend has!  He is planning on a similar build, so I built with the exact same board to avoid any surprises once he starts his build.  Supports ECC RAM.     | $229.12       |
| **CPU**                               | AMD Ryzen 5 PRO 5650G - *eBay*                                                                                                                                                                                                                        | $236.99       |
| **CPU Cooler**                        | Noctua NH-U9S CPU Cooler - *eBay*                                                                                                                                                                                                                     | $72.95        |
| **RAM**                               | OWC 32GB DDR4 3200MHz PC4-25600 CL22 2RX8 ECC Unbuffered UDIMM 1.2V 288-pin Workstation Server Memory RAM - *Amazon*                                                                                                                                  | $139.56       |
| **RAM**                               | OWC 32GB DDR4 3200MHz PC4-25600 CL22 2RX8 ECC Unbuffered Workstation Server Memory RAM - *Amazon*                                                                                                                                                     | $139.56       |
| **Storage (SSD)** for 2x cache mirror | Samsung 970 Evo Plus 2 TB M.2-2280 PCIe 3.0 X4 NVME Solid State Drive - *Canada Computers*                                                                                                                                                            | $192.09       |
| **Storage (SSD)** for 2x cache mirror | Samsung 970 Evo Plus 2 TB M.2-2280 PCIe 3.0 X4 NVME Solid State Drive - *Canada Computers*                                                                                                                                                            | $192.09       |
| **Storage (SSD)** for docker and VMs  | Western Digital Black SN850X 2 TB M.2-2280 PCIe 4.0 X4 NVME Solid State Drive - Canada Computers                                                                                                                                                      | $236.61       |
| **Storage (HDD)** parity drive        | Seagate IronWolf Pro NAS 22 TB 3.5" 7200 RPM - eBay                                                                                                                                                                                                   | $449.73       |
| **Storage (HDD)**                     | Seagate IronWolf Pro NAS 22 TB 3.5" 7200 RPM - eBay                                                                                                                                                                                                   | $449.73       |
| **Storage (HDD)**                     | Seagate Exos X14 12 TB 3.5" 7200 RPM Internal Hard Drive - already owned                                                                                                                                                                              | $0.00         |
| **Storage (HDD)**                     | Seagate Exos X14 12 TB 3.5" 7200 RPM Internal Hard Drive - already owned<br><br>Was using these Exos drives in an OWC ThunderBolt dock with my Mac as software RAID 1 mirror.<br><br>Will move this drive into the Unraid NAS after copying data off. | $0.00         |
| **Power Adapter**                     | iCAN SATA-to-Molex Power Adapter - Canada Computers                                                                                                                                                                                                   | $3.99         |
| **Power Adapter**                     | iCAN SATA-to-Molex Power Adapter - Canada Computers                                                                                                                                                                                                   | $3.99         |
| **Expansion Card**                    | PCIe x16 to x8+x4+x4 Splitter Adapter Card - [AliExpress](https://www.aliexpress.com/item/1005005277952427.html)                                                                                                                                      | $21.41        |
| **SATA Controller**                   | M.2 NVME to SATA 3.0 6 Port SATA Controller ASM1166 - [AliExpress](https://www.aliexpress.com/item/1005006394563922.html)                                                                                                                             | $24.34        |
| **Cable**                             | 0.5M 6 SATA to 6 SATA Cable - AliExpress                                                                                                                                                                                                              | $11.67        |
| **Cable**                             | 0.5M 4 SATA to 4 SATA Cable - AliExpress                                                                                                                                                                                                              | $9.32         |
| **Case Fan**                          | Noctua A9 PWM 46.44 CFM 92 mm Fan                                                                                                                                                                                                                     | $25.05        |
| **Case Fan**                          | Noctua A9 PWM 46.44 CFM 92 mm Fan                                                                                                                                                                                                                     | $25.05        |
| **Case Fan**                          | Noctua A8 PWM 32.66 CFM 80 mm Fan                                                                                                                                                                                                                     | $24.80        |
| **Case Fan**                          | Noctua A8 PWM 32.66 CFM 80 mm Fan                                                                                                                                                                                                                     | $24.80        |
| **Operating System**                  | Unraid OS Plus - Unraid                                                                                                                                                                                                                               | $120.17       |
| **USB Storage**                       | VERBATIM 98709 Store 'n' Stay USB 3.0 Nano Drive (16GB) - Amazon                                                                                                                                                                                      | $20.34        |
| **Internal USB Adapter**              | 9-pin USB Motherboard Internal Header to USB2.0 Bus Adapter Chassis Built-in Cable - AliExpress<br><br>Used this for the Unraid OS USB drive                                                                                                          | $2.74         |
| **Plug for unused USB C port**        | USB Type-C Dust Plug (for unused port) - AliExpress                                                                                                                                                                                                   | $2.88         |
| **3.5" HDD Dock** for local backup    | ORICO Tool Free 2.5'' & 3.5'' SATA HDD/SSD Docking Station with USB3.0 output - Canada Computers                                                                                                                                                      | $16.99        |
| **Storage (HDD)** for local backup    | Seagate IronWolf Pro NAS 22 TB 3.5" 7200 RPM - eBay                                                                                                                                                                                                   | $449.73       |
|                                       | Price includes shipping, taxes, etc                                                                                                                                                                                                                   |               |
|                                       | **Total**                                                                                                                                                                                                                                             | **$3,518.79** |
So I probably spent a decent bit more than I technically had to, but I'm really pleased with the build I ended up with, and it's hella future-proof!

You can check out the same list for this build on [PCPartPicker](https://ca.pcpartpicker.com/list/wjXZPF) if you'd like.

## About my 22TB HDDs

I bought "recertified" Seagate Ironwolf Pro 22TB SATA 3.5" Enterprise NAS ST22000NT001C drives on eBay.  I got 3 of these drives.

They come out to around $20 a TB.  Not the cheapest but I wanted to get some high capacity drives to start with and future proof my setup by ensuring my parity disk was as large as possible.

I am using one as a parity disk, another in the array as my first drive which is great because of its high capacity, and finally the third disk I have installed in an external HDD dock and will be using it as a local backup.

- These are supposed to be decently quiet, as far as HDDs go.
	- I found a user on [Reddit](https://www.reddit.com/r/DataHoarder/comments/10866m0/comment/j3yuhx0/?utm_source=reddit&utm_medium=web2x&context=3) who linked to some cool German website which tested a bunch of high capacity HDDs: https://www.hardwareluxx.de/index.php/artikel/hardware/storage/57753-seagate-ironwolf-pro-im-test-20-tb-durch-10-platter.html?start=3
- They tested the 20TB, the serial of the 22TB is so similar though so I assumed it would have similar characteristics...
	- After receiving them, they're not bad - but still definitely audible when they're in use.
	- My roommate said that it kind of sounds faintly like popcorn is being made :) overall kind of soothing actually.
	- I put some foam underneath the Jonsbo case which absorbs some of the higher pitched overtones which were resonating from the HDDs off the metal case onto the wooden shelf the case is sitting in.  Much better!
	- The foam I'm using is actually from the packaging the Jonsbo N3 comes with!  So, maybe save that for this purpose, if you go with the same case.
# Avoiding corruption with Unraid
Absolute power corrupts absolutely?

Okay - so, obviously having a proper backup strategy (think 3-2-1, 3 copies of you data AT LEAST, on 2 different types of media, and copy one offsite) is the most important thing.  However I was worried a bit about something more sinister... What if something "flipped a bit" on one of my HDDs storing my precious memories?  How could I notice this "bit rot" corrupting a file without it affecting my backups and poisoning my parity somehow?

Well... here's the research I did.

**Before you read, the TL;DR is:**
- The consensus seems to be that bit rot in the sense I just described it (freak bit flip on your HDD starts corrupting your data silently) is quite unlikely.
- However, RAM seems to be the most likely candidate for some sort of corruption to occur to your data during copying.
- This risk of this seems to go up the longer you have some sort of data stored in RAM.
	- which will definitely be the case if you choose to use ZFS as the ARC cache is in RAM
- So, using ECC ram which does checksumming is a good idea for a NAS build
- Also within Unraid there is a "file integrity" plugin which can check for the disk level bit rot
	- however it can't repair it - it will only notify you
	- the plugin can also throw up false warnings sometimes...
	- I decided I didn't want this headache, so I decided to use ZFS
- Use a filesystem which does checksumming like Btrfs and ZFS
	- I decided to go with a "hybrid" approach described in this article ([ZFS, Unraid Array, or Hybrid?](https://unraid.net/blog/zfs-guide)) where each disk in my array is an individual ZFS filesystem
	- ZFS does checksumming on the filesystem level
	- I got ECC RAM (and lots of it) so ZFS can do its caching thang
	- ZFS pool can do self-healing of bitrot, but with the "hybrid" approach it won't be able to do that since ZFS doesn't manage its own parity
	- if ZFS detects something is wrong on a scrub, I believe Unraid is supposed to notify me in which case I could then restore from a backup (yes you read that right.. backups!!!)

Alright... onto my notes:

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
Since we want to use the Unraid parity-protected array, ZFS pool is not an option (but we can still do individual ZFS file systems on each array disk...).

It has advantages, such as snapshots, compression and self-healing of bitrot.

ZFS pools do have drawbacks.  One being, like any other RAID array, if you lose more disks than parity, you actually lose ALL your data, as a consequence of it being striped across all disks in the pool.

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