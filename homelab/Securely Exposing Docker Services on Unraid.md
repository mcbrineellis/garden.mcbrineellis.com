---
created: 2024-09-09T22:22
updated: 2024-10-02T16:21
---
This is sort of my continuation of the [[Captain's Log]] page.  Gonna split these out into individual #blog pages from now on.

Some unrelated random links to start this post off:

- I want to learn more about OpenTelemetry with Grafana: https://grafana.com/go/webinar/mastering-code-based-instrumentation-with-opentelemetry-and-grafana/
- A coworker recommended this recipe manager app: https://mealie.io/
- PC case finder website: https://caseend.com/

Got a couple things I'd like to accomplish on my homelab:

- Clean up my Docker networking and set up internal/external access
- Set up a new Jellyfin server and make it so my family can access it.

## Domain names (not ports!) for Unraid Docker containers
Inspired by: https://www.ericjstauffer.com/blog/local-domain-names-for-unraid-docker-containers-with-port-numbers and https://mitchross09.medium.com/how-i-self-host-my-own-sites-and-applications-with-unraid-docker-authelia-and-cloudflare-1f0d8e6f8912

Currently all of my stuff is accessed like this (my Unraid server is called consbo...):
```
http://consbo.csilo.ca:8384
http://consbo.csilo.ca:8000
http://consbo.csilo.ca:9000
```

Until now, if I wanted to expose things to the internet, I was using an `Unraid-Cloudflared-Tunnel` container and then using the Docker IP+Port of my service (like above) as the backend.  Then I could set up a URL like `https://server.mcbrineellis.com` that was publicly accessible on the Cloudflare end.  And to access internally, I was stuck with using `consbo.csilo.ca` and remembering the port numbers.

- I don't want to have to enter port numbers
- I want separate FQDNs for each of my services, internal OR external

To accomplish that, I'd like to have Traefik/Nginx Proxy Manager or other similar proxy listening on 80/443 (internally), handling SSL and proxying the traffic to my stuff.

Then I can set up DNS A records pointing to the Unraid IP and the proxy will redirect my traffic to the correct container using SNI matching.

### Configuring the network

~~Under **Settings** > **Network Settings**, I turned on **Enable VLANs** and created a new interface with the VLAN number 42 (my home lab is on a separate VLAN to the rest of my home network).~~

~~Then I went to **Settings** > **Docker**, temporarily disabled Docker, then checked the box under **IPv4 custom network on interface br0.42 (optional):** to turn on the custom network.~~  

**Update:** I ended up creating a separate custom network for Docker, by opening the Unraid console and running `docker network create tayne`.  Then, under **Settings** > **Network Settings** I set *Preserve user defined networks* to Yes so it won't be wiped out on a reboot.  I also set **Host access to custom networks** to Enabled, set **Enable Docker** back to Yes, and clicked apply.

### Installing Nginx-Proxy-Manager-Official

I used the `Nginx-Proxy-Manager-Official` template in the Community Applications store.

Set the network to `Custom : tayne` and gave a static IP of 172.21.12.4
Left all the other defaults, and then added the container and logged into the NPM interface on port 81 via http://172.21.12.4:81/ with the default login of `admin@example.com` and password `changeme`.

**Docker does not support automatic service discovery on the default bridge network.** If you want to communicate with container names in this default bridge network, you must connect the containers via the legacy docker run --link option.


## New Jellyfin Server

The current image that I'm using is from LinuxServer.io [linuxserver/jellyfin](https://docs.linuxserver.io/images/docker-jellyfin/).

There's now an official Jellyfin Docker image, so I think it probably makes sense to use that instead.

I also found this cool new Jellyfin client: https://github.com/fredrikburmester/streamyfin

It's time to finish up what I started a couple weeks ago.