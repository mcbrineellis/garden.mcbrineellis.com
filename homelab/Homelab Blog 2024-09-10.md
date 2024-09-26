---
created: 2024-09-09T22:22
updated: 2024-09-10T12:36
---
This is sort of my continuation of the [[Captain's Log]] page.  Gonna split these out into individual #blog pages from now on.

Got a couple things I'd like to accomplish on my Homelab:

- Set up a new Jellyfin server and make it so my family can access it.
- Clean up my Docker networking and change it from a bridge to a routed VLAN network that is accessible from my Proxmox hosts as well?

## New Jellyfin Server

The current image that I'm using is from LinuxServer.io [linuxserver/jellyfin](https://docs.linuxserver.io/images/docker-jellyfin/).

There's now an official Jellyfin Docker image, so I think it probably makes sense to use that instead.