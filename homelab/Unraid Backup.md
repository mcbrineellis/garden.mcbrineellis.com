Now that I've got my [[Consbo N3 NAS Build]] finished, I need to get all my data backed up.

So far I've got a spare 22TB HDD set up as an unassigned device ready to back up to, and I'm comparing different cloud storage options for offsite backup.
## Cloud Storage
I want to have an offsite copy of my data, but I don't want to break the bank.  Here is how I have broken down the options for cloud storage.

- **S3-compatible object storage providers**
	- Amazon S3
		- I have some stuff in a Glacier archive
	- iDrive e2
	- Backblaze B2
		- I was testing out B2 and have a small handful of files stored there
	- Wasabi
- **Cloud drives / storage boxes**
	- Google, Dropbox, iCloud etc
		- Works fine as hot storage so performance will be good
	- MEGA
		- Pro III plan is 16 TB storage and 192 TB transfer for $438.71 CAD /year (€299.99)
		- Not sure I trust this company?
	- Hetzner Storage Box
		- Great prices, very attractive option but I expect the transfer speed will be slow
		- Located in Germany, which is actually a plus for me since I live in Canada and would like my offsite backup to be far away from home
	- Some other options like these listed on this [comparison table](https://www.reddit.com/r/DataHoarder/comments/1aketpm/i_made_a_huge_comparison_table_to_help_you_find/) I found on Reddit.
- **Storage VPS / Storage Servers**
	- Some of them I found seem like they are just resellers of Hetzner services...
	- Hostbrr
	- HostHatch
	- GreenCloud
	- Crunchbits
	- Contabo
	- Alphavps
	- ...and a billion other similar providers on [LowEndBox](https://lowendbox.com/) / [LowEndTalk](https://lowendtalk.com/)
- **Endpoint Backup Services**
	- These are sometimes marketed as "unlimited" but might come with hassles.  Some people abuse them and throw up many TBs of data but it might be hard to restore and download it all... you're also probably stuck using the company's official backup client in order to use the service or obtain your data.
	- Crashplan
		- Saw lots of Unraid users talking about how they use this.
		- Crashplan is in the Unraid app store.
	- Backblaze
		- Some folks used [VirtioFS](https://www.reddit.com/r/unRAID/comments/1b50t9q/virtiofs_is_now_stable_under_windows/) to back up their data using the Backblaze official client presenting the Unraid shares to Windows as a regular disk.
		- Seems like a bit of a grey area in terms of TOS compliance... but in online forums some of their staff [acknowledged this usage](https://www.reddit.com/r/backblaze/comments/11kuz88/comment/jbaiq07) and seemed to ["bless" it](https://www.reddit.com/r/backblaze/comments/11kuz88/comment/jbd0ufm).  However it is still slightly sketchy in my view.  Maybe I would set this up as a secondary offsite backup "just in case".
## Backup Software
For actually backing up the config, Appdata Backup plugin in Unraid looks promising.  Can back up VMs and Appdata to a folder, and then have my backup tool of choice back that folder up.

- **Duplicacy**
	- Paid software but very good reviews, has a nice GUI interface and supports a bunch of different cloud storage as well as local drives.
	- The CLI part is open source and totally free.
- **Kopia**
	- Newer solution, people seem to like it, but maybe a bit unproven?
	- https://kopia.io/docs/features/#verifying-backup-validity-and-consistency
	- Need to do more research on this one.
	- Has a GUI.
- **Duplicati**
	- Has a bad rap for being unreliable and the DB getting corrupted.  Sounds pretty featureful though, so could be interesting to try out.
	- Need to do more research on this one.
	- Has a GUI
- **Restic**
	- This guy uses Restic together with Rclone with some scripts:
		- https://github.com/DavidKrGH/BackupScripts
		- https://forums.unraid.net/topic/140004-backup-scripts-combining-restic-and-rclone/
	- Jim's Garage video on Docker backup with Restic:
		- https://www.youtube.com/watch?v=WBBTC5WfGis
	- No GUI
- **Borg / Borgmatic**
	- Borg requires its own server on the remote host (Borgserver?)
	- So this is a no-go unless I have a VPS or something on the remote end and not just some dumb storage or bucket
		- Or can use a service like [BorgBase](https://www.borgbase.com/)
	- Tutorial someone wrote for using Borg with RClone and some shell scripts
		- https://www.reddit.com/r/unRAID/comments/e6l4x6/tutorial_borg_rclone_v2_the_best_method_to/
- **LuckyBackup**
	- GUI backup container using rsync on the backend
	- https://unraid.net/blog/unraid-server-backups-with-luckybackup

Special mention (cloud storage abstraction and file syncing):
- **Rclone**
	- Awesome tool that supports a ton of different cloud storage and abstracts that part away.  Some people even use it alone to run their backups.
- **Syncthing**
	- File syncing tool, could use it to sync data to another Unraid server perhaps.
## Duplicacy
So I decided to give Duplicacy a go, since I saw so many people saying good things about it reading through the forums and on Reddit.

I installed Duplicacy from the ["Selfhosters" Unraid Repo](https://github.com/selfhosters/Unraid-CA-templates), which uses the [saspus/duplicacy-web](https://hub.docker.com/r/saspus/duplicacy-web) docker image.
### Docker Image Config
I adjusted the config:
- Toggled "advanced view" in the top right corner
- Updated the hostname under extra parameters
- Set the value for **User Data** to `/mnt/user` which will map to `/backuproot` in the container.
- That's it!  Clicked apply, and it ran:
```
## Pulling image: saspus/duplicacy-web:latest

IMAGE ID [518773564]: Pulling from saspus/duplicacy-web.  
IMAGE ID [4abcf2066143]: Pulling fs layer. Downloading 100% of 3 MB. Verifying Checksum. Download complete. Extracting. Pull complete.  
IMAGE ID [c974b6a1a6bf]: Pulling fs layer. Downloading 100% of 18 MB. Verifying Checksum. Download complete. Extracting. Pull complete.  
IMAGE ID [c8965f97dee5]: Pulling fs layer. Downloading 100% of 943 B. Download complete. Extracting. Pull complete.  
Status: Downloaded newer image for saspus/duplicacy-web:latest  
  
**TOTAL DATA PULLED:** 21 MB  

## Command execution

docker run  
  -d  
  --name='Duplicacy'  
  --net='bridge'  
  -e TZ="America/Los_Angeles"  
  -e HOST_OS="Unraid"  
  -e HOST_HOSTNAME="consbo"  
  -e HOST_CONTAINERNAME="Duplicacy"  
  -e 'USR_ID'='99'  
  -e 'GRP_ID'='100'  
  -l net.unraid.docker.managed=dockerman  
  -l net.unraid.docker.webui='http://[IP]:[PORT:3875]/'  
  -l net.unraid.docker.icon='https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/duplicacy.png'  
  -p '3875:3875/tcp'  
  -v '/mnt/user/':'/backuproot':'rw'  
  -v '/mnt/user/appdata/Duplicacy':'/config':'rw'  
  -v '/mnt/user/appdata/Duplicacy/cache':'/cache':'rw'  
  -v '/mnt/user/appdata/Duplicacy/logs':'/logs':'rw'  
  --hostname=consbo 'saspus/duplicacy-web'  
  
**The command finished successfully!**
```

I noticed that in the above output it used the America/Los_Angeles timezone (I'm in Eastern), so I went into **Settings** > **Date and Time** and fixed my timezone setting, then restarted Duplicacy.

I installed **Unassigned Devices** from the app store, and I mounted the external HDD I plan on using for backups.

Then I went back and edited the Docker config, adding a new volume mapping:
- Config Type: `Path`
- Name: `Backup HDD`
- Container Path: `/mnt/disks/backuphdd`
- Host Path: `/mnt/disks/backuphdd`

Docker did its thang:
```
docker run  
  -d  
  --name='Duplicacy'  
  --net='bridge'  
  -e TZ="America/New_York"  
  -e HOST_OS="Unraid"  
  -e HOST_HOSTNAME="consbo"  
  -e HOST_CONTAINERNAME="Duplicacy"  
  -e 'USR_ID'='99'  
  -e 'GRP_ID'='100'  
  -l net.unraid.docker.managed=dockerman  
  -l net.unraid.docker.webui='http://[IP]:[PORT:3875]/'  
  -l net.unraid.docker.icon='https://raw.githubusercontent.com/selfhosters/unRAID-CA-templates/master/templates/img/duplicacy.png'  
  -p '3875:3875/tcp'  
  -v '/mnt/user/':'/backuproot':'rw'  
  -v '/mnt/disks/backuphdd':'/mnt/disks/backuphdd':'rw'  
  -v '/mnt/user/appdata/Duplicacy':'/config':'rw'  
  -v '/mnt/user/appdata/Duplicacy/cache':'/cache':'rw'  
  -v '/mnt/user/appdata/Duplicacy/logs':'/logs':'rw'  
  --hostname=consbo 'saspus/duplicacy-web'
```
### Adding Storage
Then I opened into the Duplicacy web UI, and set my config password (saved it in 1Password).

There are a couple guides I referenced for this part:
- https://duplicacy.com/guide.html
- https://forum.duplicacy.com/t/duplicacy-user-guide/1197

I started by configuring the HDD under the storage tab.

Storage configuration:
- Disk
	- Directory `/mnt/disks/backuphdd`
- Storage Name: `local`
- Password (saved it in 1Password)
- Left rest as defaults (LZ4 compression), but I turned on 5:2 erasure coding.
### Adding Backup Jobs
Finally we can add our first job!  Open the Backup tab.
I added my license in already under this page but if you haven't done that yet, you can do it now.

After clicking the + sign to add a new backup, we get a simple dialog box.
- Directory: `/backuproot/isos`
- Storage: `local` (this is the storage we just added in)
- Backup ID: `isos`

After adding the job, I clicked run - and it just went ahead and did its thing!  Nice!

A couple helpful Reddit threads with people sharing what they are backing up on Unraid:
- https://www.reddit.com/r/unRAID/comments/1ba1yo9/what_folders_do_i_back_up_with_duplicacy/
- https://www.reddit.com/r/unRAID/comments/16qx7ew/what_do_you_use_to_backup_unraid/
### Adding Off-Site Copy Jobs
This is just a matter of adding a remote storage target, and adding a copy job from local>remote to your schedules!

I signed up for a Hetzner 10TB storage box, added it as an SFTP target, and configured it as:
- Copy-compatible with `local` (my local backup repo)
- Compression set to `zstd (best)`

After setting `-threads 10` copy speeds to Germany are around ~21MB/s (170Mbps), which is totally acceptable performance for an offsite copy!  Copying 10TB offsite at this speed should take around 5.5 days.
### Backup Restore
Restoring data is handled through the GUI.  I did some quick searches and it sounds like it's not possible to mount a backup as if it were a filesystem, which is disappointing.  Hopefully they will add that in the future.
## Unraid System Backup
So, the idea here is to use various plugins to back up the Unraid system itself, and then have Duplicacy pick up those backup files and back them up to your offsite storage etc.
### Appdata Backup
I installed the Appdata Backup plugin from the app store.
It's terrifically simple.  You just provide it a path to back up to.

I created a share specifically for these backups, which I will then in turn back up with Duplicacy.

I realized that Appdata Backup is creating tar files which means perhaps the Duplicacy dedupe might not work properly.  Will have to look into a better solution long term perhaps.