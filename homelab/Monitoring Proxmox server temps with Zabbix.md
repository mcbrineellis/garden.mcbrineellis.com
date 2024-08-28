---
created: 2024-08-26T21:05
updated: 2024-08-28T15:37
---
I want to be sure that my servers aren't overheating.  So, let's monitor the temps!

```
root@pve1:~# apt install lm-sensors
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libsensors-config libsensors5
```

After installing `lm-sensors` now I can see a bunch of cool temperature information:

```
root@pve1:~# sensors
coretemp-isa-0000
Adapter: ISA adapter
Package id 0:  +42.0°C  (high = +84.0°C, crit = +100.0°C)
Core 0:        +38.0°C  (high = +84.0°C, crit = +100.0°C)
Core 1:        +39.0°C  (high = +84.0°C, crit = +100.0°C)
Core 2:        +40.0°C  (high = +84.0°C, crit = +100.0°C)
Core 3:        +39.0°C  (high = +84.0°C, crit = +100.0°C)

acpitz-acpi-0
Adapter: ACPI interface
temp1:        +27.8°C  (crit = +119.0°C)
temp2:        +29.8°C  (crit = +119.0°C)

pch_skylake-virtual-0
Adapter: Virtual device
temp1:        +54.5°C  
```

Using the [Sensor item type](https://www.zabbix.com/documentation/current/en/manual/appendix/items/sensor) in Zabbix I can now monitor these temperatures via the Zabbix Agent installed on this server.