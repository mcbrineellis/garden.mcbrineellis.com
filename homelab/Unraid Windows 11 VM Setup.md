---
created: 2024-08-27T18:23
updated: 2024-08-27T19:03
modified: 
---
First I downloaded the install ISOs:

- [Windows 11 Disk Image](https://www.microsoft.com/en-ca/software-download/windows11) 
- [Virtio Drivers and Guest Tools](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/)

In Unraid under **VMs** > **Add VM** I chose Windows 11.

I made these adjustments to the default settings, YMMV:

- Set Autostart to `YES`
- Set the name to an FQDN with my domain
- Assigned CPU2-11 (4 cores, 8 threads)
- Set initial/max memory to 12288 MB
- Set the USB Controller to 3.0 (nec XHCI)
- OS Install ISO: `Win11_23H2_English_x64v2.iso`
- VirtIO Drivers ISO: `virtio-win-0.1.262.iso`
- Primary vDisk Size: `120G`
- Set the unraid share to my media share, `User:media`

After starting the installer, no storage will be found due to the missing virtio network driver:

- Open the virtio CD, browse to `amd64\w11`, click OK
- *Hide drivers that aren't compatible with this computer's hardware* should be checked
- Install the driver that shows up and then your virtual disk should appear.  Click next to proceed with install.

When first booting up into the OOBE after install, I got stuck at the "Let's connect you to a network" screen.  This is because the virtio network drivers haven't been installed yet.  Typical Microsoft nonsense to hide the bypass option.

`Shift+F10` will open a Command Prompt and from there you can can enter `OOBE\BYPASSNRO`.  The computer will restart and ask again to confirm your Country and Keyboard layout.

Now it will show an "I don't have internet" option to bypass the network setup, followed by "Continue with limited setup" on the next screen which seems a bit redundant...

After booting into the newly installed system, open the CD drive E: and run `virtio-win-guest-tools.exe` to install the rest of the guest tools and drivers.

To access the Unraid [virtio-fs](https://virtio-fs.gitlab.io/howto-windows.html) share we need to install [WinFsp - Windows File System Proxy](https://winfsp.dev/):

- Download and run the installer from the WinFsp site
- Open `services.msc` and find `VirtIO-FS Service`
- Right click, open Properties
- Set Startup type to Automatic and click Start
- Click OK

Open File Explorer and check that the shared drive is showing up!

All done!  Don't forget to set the hostname on your system.  Next, I connected up a Blu-Ray drive and ripped a bunch of DVDs using MakeMKV.