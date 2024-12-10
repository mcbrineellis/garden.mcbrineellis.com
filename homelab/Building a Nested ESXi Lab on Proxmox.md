---
created: 2024-11-24T13:19
updated: 2024-11-25T20:29
---
I followed the link on [William Lam's website](https://williamlam.com/nested-virtualization) to the new [Broadcom Flings](https://brcm.tech/flings) portal.

There I downloaded the latest release (8.0U3) under **Nested ESXi Virtual Appliance**, which gave me a file `Nested_ESXi8_0u3b_Appliance_Template_v1_ova-dl.zip`.

I scp'd the file over to my Proxmox server: `scp ~/Downloads/Nested_ESXi8_0u3b_Appliance_Template_v1_ova-dl.zip root@cmb1-pve:~/`

Then I installed unzip, and unzipped & untarred the OVA files:
```
root@cmb1-pve:~# apt install unzip
root@cmb1-pve:~# unzip Nested_ESXi8_0u3b_Appliance_Template_v1_ova-dl.zip
root@cmb1-pve:~# tar xvf Nested_ESXi8.0u3b_Appliance_Template_v1.ova
Nested_ESXi8.0u3b_Appliance_Template_v1.ovf
Nested_ESXi8.0u3b_Appliance_Template_v1.mf
Nested_ESXi8.0u3b_Appliance_Template_v1-disk1.vmdk
Nested_ESXi8.0u3b_Appliance_Template_v1-disk2.vmdk
Nested_ESXi8.0u3b_Appliance_Template_v1-disk3.vmdk
```

You can see the tar command unzipped the OVF (VM configuration), MF (checksum info), and VMDK (disk files) contained inside of the OVA.

Next we'll import the VM configuration using the `qm` command.  I tried importing using the qcow2 format for the disks but it didn't work and instead the disks were imported as raw.

```
root@cmb1-pve:~# qm importovf 201 ./Nested_ESXi8.0u3b_Appliance_Template_v1.ovf local-lvm --format qcow2
format 'qcow2' is not supported by the target storage - using 'raw' instead
  Logical volume "vm-201-disk-0" created.
transferred 0.0 B of 16.0 GiB (0.00%)
transferred 163.8 MiB of 16.0 GiB (1.00%)
transferred 327.7 MiB of 16.0 GiB (2.00%)
...
cut!
...
transferred 16.0 GiB of 16.0 GiB (100.00%)
format 'qcow2' is not supported by the target storage - using 'raw' instead
  Logical volume "vm-201-disk-1" created.
transferred 0.0 B of 4.0 GiB (0.00%)
transferred 4.0 GiB of 4.0 GiB (100.00%)
format 'qcow2' is not supported by the target storage - using 'raw' instead
  Logical volume "vm-201-disk-2" created.
transferred 0.0 B of 8.0 GiB (0.00%)
transferred 8.0 GiB of 8.0 GiB (100.00%)
```

Then I could see that the VM appeared in my Proxmox:
*Virtual Machine 201 (NestedESXi8.0u3bApplianceTemplate) on node 'cmb1-pve'*

I added a VMXNET3 NIC as I assume that would be the most compatible.

Then I powered it up!  and immediately got an error message:

```
VMB: 737:
Unsupported CPU: Intel family 0x0f, model 0x06, stepping 0x1
Common KVM processor
See http://www.vmware.com/resources/compatibility
```

I then found this article (which is awesome!  definitely check it out for more info on nested ESXi on Proxmox) https://iriarte.it/homelab/2023/09/05/esxi-on-proxmox-as-nested-hypervisor.html and based on their settings I made these changes:
- Changed the CPU type on the VM from "Default (kvm64)" to "host"
- Changed the SCSI Controller from "Default (LSI 53C895A)" to "VMware PVSCSI"

And then everything booted up fine!

The root password for the images is `VMware1!`

Then I configured the NIC I added to each host.

The resulting ESXi hosts had no storage, so I needed to figure out a way of providing them storage.  I decided on setting up a FreeNAS 