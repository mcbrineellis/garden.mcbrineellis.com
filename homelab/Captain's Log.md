---
created: 2023-12-07T17:26
updated: 2024-07-04T14:03
---
### Thu July 4, 2024
A photographer friend had a problem where a single photo on one of their SD cards got corrupted.  I wrote up a how-to on [[Testing SD Card Health and Speed with f3]].

Was curious what folks do to test HDD or other SMART-compatible storage, and found [this comment](https://www.reddit.com/r/DataHoarder/comments/f0goa1/comment/fgubmrn/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button):

> This is something I developed to stress both new and used drives so that if there are any issues they will appear.  Testing can take anywhere from 4-7 days depending on hardware. I have a dedicated testing server setup.
> 
> I use a server with ECC RAM installed, but if your RAM has been tested with MemTest86+ then your are probably fine.
> 
> **1) SMART Test, check stats**
>     smartctl -A /dev/sdxx
>     smartctl -t long /dev/sdxx
> 
> **2) BadBlocks -This is a complete write and read test, will destroy all data on the drive**
>     badblocks -b 4096 -wsv /dev/sdxx > $disk.log
> 
> 3) **Format to ZFS** -Yes you want compression on, I have found checksum errors, that having compression off would have missed. (I noticed it completely by accident. I had a drive that would produce checksum errors when it was in a pool. So I pulled and ran my test without compression on. It passed just fine. I would put it back into the pool and errors would appear again. The pool had compression on. So I pulled the drive re ran my test with compression on. And checksum errors. I have asked about. No one knows why this happens but it does. This may have been a bug in early versions of ZOL that is no longer present.)
> 
>     zpool create -f -o ashift=12 -O logbias=throughput -O compress=lz4 -O dedup=off -O atime=off -O xattr=sa TESTR001 /dev/sdxx
>     zpool export TESTR001
>     sudo zpool import -d /dev/disk/by-id TESTR001
>     sudo chmod -R ugo+rw /TESTR001
> 
> **4) Fill Test using F3**
>     f3write /TESTR001 && f3read /TESTR001
> 
> **5) ZFS Scrub to check any Read, Write, Checksum errors.**
>     zpool scrub TESTR001
> 
> Edit: You can combine steps 4 and 5 by doing this in root
>     f3write /TESTR001 && f3read /TESTR001 && zpool scrub TESTR001
> 
> If everything passes, drive goes into my good pile, if something fails, I contact the seller, to get a partial refund for the drive or a return label to send it back. I record the wwn numbers and serial of each drive, and a copy of any test notes
> 
>     8TB wwn-0x5000cca03bac1768 -Failed, 26 -Read errors, non recoverable, drive is unsafe to use.
>     8TB wwn-0x5000cca03bd38ca8 -Failed, CheckSum Errors, possible recoverable, drive use is not recommend.
### Wed Jun 26, 2024
Finally reinstalled IDM!
### Tue June 20, 2024
https://www.scrypted.app/
### Tue June 18, 2024
Discovered cool thing:
https://docs.paperless-ngx.com/

### Mon Jun 17, 2024
Time to get working on my homelab again!!!

Gonna decommission the idm server since it was set up with secure boot EFI and the migration from ESXi to Proxmox isn't working, I don't want to bother with spending the time to troubleshoot it.

```
172.21.12.1   idm.home.mcbrineellis.com
172.21.12.2   vcsa
172.21.12.3   adguardhome
172.21.12.10  rancher
172.21.12.11  k8s-node1-ctrl
172.21.12.12  k8s-node2
172.21.12.13  k8s-node3
172.21.12.41  veeam
172.21.12.90  dev
172.21.12.91  media
172.21.12.254 pfsense
192.168.2.63  homeassistant
192.168.2.201 esx1 / pve1
192.168.2.202 esx2 / pve2
192.168.2.203 pve3
```

### Tue Apr 2, 2024
Finally got the last of my data clear off my OWC dock and the disks installed into Consbo.  It's doing the disk-clear step.
Left to do:
- [x] Get my first backup done
- [x] Get my first offsite copy done
- [x] Run a parity check
- [x] Adjust my remote backup so that copy is "airgapped"
	- [x] separate stuff that changes often from archival / bulk WORMy data like photos & videos

Fixed a networking issue I was having with my Proxmox hosts... I am using USB 2.5GbE NICs and I had them connected and on the same subnet as the 1GbE NICs built into the hosts.  Even with the default gateway on the 2.5GbE bridge interface, traffic seemed to always flow out of the 1G NIC.  To solve this seeming non-deterministic routing problem, I just adjusted the 1GbE NICs to run on a different subnet (which I can change to in case I lose access)

Migrating my pfSense VM from ESXi to PVE.  Did a config export, rebuilding the VM then importing.
### Sun Mar 31, 2024
- Did a ton of research and finally got my [[Unraid Backup]] set up!
- Going to shuck an 8TB Seagate Expansion HDD I have lying around and throw that into my array (maybe as a separate pool or unassigned tho, since it's SMR)
- Reading this Reddit post [How I Left The Cloud](https://www.reddit.com/r/selfhosted/comments/196nke8/how_i_left_the_cloud/) on r/selfhosted
- Watching the latest LTT video [Paying for Cloud Storage is Stupid](https://www.youtube.com/watch?v=QsM6b5yix0U)
- Debating buying Neewer wireless mics for filming an upcoming trip...
	- Which got me looking up the latest Zoom F3 recorder, and then I saw this which looks NEAT AS HECK... https://immersivesoundscapes.com/earsight-thumb-microphones/-p502321903
		- linked on this thread: https://gearspace.com/board/studio-stage-mobile-amp-location-equipment/1408033-mics-turn-zoom-f3-into-portable-field-recorder.html
	- still dreaming of the ultimate portable / stealth field recording setup I could take into concert venues and get awesome capture of the show
- Gmail was launched 20 years ago!  https://lowendtalk.com/discussion/193820/how-time-flies-20-years-since-gmail-launch#latest
### Fri Mar 29, 2024
Totally revamped and improved my article on my [[Consbo N3 NAS Build]]!  But, I should really go to sleep, it's 2 AM...
### Mon Mar 25, 2024
Thinking about how I might set up Unraid backup, now that I've build my Unraid system.
- Backing up iCloud Photos to Unraid: https://www.youtube.com/watch?v=9UJBkxg9tCo
- Cloudberry Backup?
### Thu Mar 14, 2024
Back at it with some homelab work.
- Expanded the disk on my Ubuntu VM on pve2 handling the export and import between esx1 and pve3.
### Wed Feb 28, 2024
I reinstalled 2 of my 3 nodes to Proxmox.  First impressions are really good!
https://www.smarthomebeginner.com/proxmox-ssl-certificate-with-letsencrypt/
https://www.youtube.com/watch?v=nL0x0LIpLlk
Seeing some weirdness on my 2.5GbE NICs.  Going to have to play around with that.

Had to come up with a naming scheme since VMIDs are so prevalent in Proxmox.  Here's what I came up with for now. 

```
Left 100-299 free.


301 pfsense
302 idm
303 adguardhome
330 zabbix
341 veeam

401 dev
402 vcsa

501 homeassistant
502 media
503 mine

```
### Sat Dec 23, 2023
So, I got my IdM set up, and I messed around with public and private DNS a bunch.
Installed Rancher.  Installed VCSA (freakin' BLOAT!  Do I really need this???)

Alright!  After much ado, my cluster is deployed!

> Helm Charts is to Kubernetes what Docker-Compose is to Docker

This is a good piece of info.

https://blog.connley.net/2023/01/16/easy-static-ip-configuration-for-rancher-nodes/
Hmm... is it okay that my nodes have DHCP addresses?  Gonna say.... no.

*Fine, let's set up this Network Protocol Profile biznozz:*
1. Navigate to a data center that is associated with a vApp. 
2. On the Configure tab, select More > Network Protocol Profiles. 
    Existing network protocol profiles are listed. 
3. Click the Add button. 
    The Add Network Protocol Profile wizard opens.

Okay, cool.

![[2024-01-04 Chinese w Eddie.jpg]]

https://technotim.live/posts/kube-traefik-cert-manager-le/
https://www.youtube.com/watch?v=G4CmbYL9UPg - traefik
### Fri Dec 22, 2023
So anyways, I came across Rancher today!  I saw this post on Reddit: [Kind of excited that I finally went this route (rancher for k8s)](https://www.reddit.com/r/homelab/comments/18lz733/kind_of_excited_that_i_finally_went_this_route/) and then watched a video called [Kubernetes Cluster in Minutes in VMware vSphere using Rancher](https://www.youtube.com/watch?v=lKzkgmR6A2A) and was sold.  The setup seems delightfully simple.  Simply deploy the Rancher docker container on your system, point it at an ESXi host and it will go ahead and deploy your Kubernetes cluster for you!

More robust than a single node setup, and it has pretty dashboard and is well supported?  Dope.
#### Rancher's [Quick Start](https://www.rancher.com/quick-start)
##### Step 1: Prepare a Linux Host
Prepare a Linux host with any supported Linux distribution including  [openSUSE](https://www.opensuse.org/) and at least 4GB of memory. Install a [supported version of Docker](https://rancher.com/docs/rancher/v2.x/en/installation/requirements/?_gl=1*1u09mn1*_ga*NTI0MDAzNTU3LjE3MDMyMTUyNTM.*_ga_Y7SFXF9L00*MTcwMzIxNTI1My4xLjEuMTcwMzIxNTI1OS41NC4wLjA.#operating-systems-and-container-runtime-requirements) on the host.
##### Step 2: Start the server
To install and run Rancher, execute the following Docker command on your host:
```
$ sudo docker run --privileged -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
```
To access the Rancher server UI, open a browser and go to the hostname or address where the container was installed. You will be guided through setting up your first cluster. To get started quickly, have a look at out additional resources and getting started guide.

Okay, so that seems doable.

I'll figure out certificates etc later...
First of all though I want to make sure my DNS is set up properly.  So, I'm deploying Red Hat IdM.

I want to set up [ArgoCD](https://www.reddit.com/r/selfhosted/comments/10x7ayj/did_someone_say_overengineering/) and Authentik later as well.

Ok back on topic.  Here's the Red Hat docs on [Installing an IdM server: With integrated DNS, with an integrated CA as the root CA](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/installing_identity_management/installing-an-ipa-server-with-integrated-dns_installing-identity-management).
### Thu Dec 21, 2023
I tried setting up Microk8s on Ubuntu, which led me down a rabbit hole.  I though that maybe I want something prettier... with a dashboard, and more robust... so I can play around with multiple nodes.  Both Microk8s and its competitor K3s - which I believe we use at ThinkOn - seem a bit too bare bones.  You can make a cluster with Microk8s but that's not its primary intended usage.

I completed a little training course on Kasten's website called [[Kube Campus - Reviewing Kubernetes Concepts and Building a Cluster Lab]] which was fun.  A good refresher since it's been about a year and a half since I did my course on Red Hat OpenShift.

So anyways... back to my search for [[Kubernetes Distributions]]... I considered deploying OpenShift, but I think I'd like to try something different.
