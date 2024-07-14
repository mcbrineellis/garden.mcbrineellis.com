---
created: 2024-07-14T18:55
updated: 2024-07-14T19:28
---
I like thinking about photography workflows as I've never quite settled on a single approach in my time so far.  I think I'm getting to close to my dream setup though.

## Database vs Filesystem

I've used the following tools over the years (non exhaustive list):
- iPhoto / Apple Photos
- Adobe Lightroom ("Lightroom Classic")
- Adobe Lightroom Cloud
- Capture One
- Google Photos
- Fujifilm X RAW Studio
- Picasa

I think photo management tools can generally be split into two camps:

**1. Photos sit in a database along with metadata and edits**

This means the photos are ingested into the app, and the only way to get photos out is to run an export.  This creates a situation where you are effectively locked in to the tool and other software cannot access your photos unless they talk to the app via its APIs which may greatly limit the number of apps which you can use unless they were specifically coded to work with your software.  For example, when you are using Apple Photos, Final Cut Pro has a specific integration that allows you to browse your photo albums, but DaVinci Resolve has no such thing.

- iPhoto / Apple Photos
- Google Photos
- Adobe Lightroom Cloud

**2. Photos sit in the filesystem and metadata is stored in a catalog or a sidecar file**

The files sit in the filesystem organized in a folder hierarchy according to the user's preferences.  The folders could be something like Year > Date + Event Name.

This allows you to easily use multiple apps to edit, organize or backup your photo files based on your needs, without having to commit to one platform or another.  This is especially important if you are using a NAS to centrally store and manage the backup of your photo files.

- Adobe Lightroom ("Lightroom Classic")
- Capture One
- Picasa
- Fujifilm X RAW Studio

## Workflow Tools

Now that I've built an Unraid NAS, I can use self-hosted tools for backup, editing and publishing, reducing my dependence on services like Apple or Google Photos which might be convenient, but are actually rather limiting.
### Backup

Backing up photos from mobile device or camera to the NAS.

For iOS devices:
- Photo Sync App
- Immich

For MacOS Devices:
- SyncThing
### Editing and organizing

Consbo Filesync + Editor / Organizer app.

- digiKam
- Lightroom
- Capture One
- Fujifilm X RAW Studio
- FastRawViewer
	- I saw this tool recommended in this thread about [DAM for Fujifilm RAW+JPEG](https://www.dpreview.com/forums/thread/4594548?page=2)
- Nomacs
- XnView
- https://www.reddit.com/r/macapps/comments/mmgltt/mac_equivalent_to_irfanview/
### Publishing

Taking photos and easily sharing them to a self hosted photo gallery or a social network.

- Flickr
- Pigallery2
- Immich (need to expose to the internet securely?)

## Folders, Tags

> Tag management takes place in a photo management software (lightroom, for example), but with tags integrated into the exif data.
> 
> As a result, a tag-based organization would have been much richer and more flexible. If you want to have a photo in several themes in a folder-based organization, you have to duplicate the photo. In a date-based organization with tags, a photo can have several tags, and therefore belong to several themes via a simple search.
> 
> But the aim is also to have as little adherence as possible to one solution or another, because times, they are a changin', fashions pass, products may no longer be maintained , and it's important not to be captive.
> 
> https://www.reddit.com/r/immich/comments/14ydgvj/comment/jrundkx/

I really like this advice.

> I think for me the general point is that any app will not sustain the test of time. None have, so far, survived the last 25 years i have been storing digital photos. So keeping photos on filesystem, sorted by folders, with metadata inside tags OR sidecar files, has been the only long term reliable format, and i plan to keep using this approach. Either Immich can support (and the first step is a real folder view) it cannot be anything really better than Google Photos (except, of course, privacy).
> 
> [Comment on Github Discussion - Immich - [Feature] Folder view #5409](https://github.com/immich-app/immich/discussions/5409#discussioncomment-9764167)

# Escaping the Walled Gardens

## Google Photos

- Google provides a method to download all photos using Google Takeout.
- [immich-go](https://github.com/simulot/immich-go) takes a Google Takeout export and imports it to Immich.
## iCloud Photos

https://github.com/boredazfcuk/docker-icloudpd