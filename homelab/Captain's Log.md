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

![[image.jpg]]

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
