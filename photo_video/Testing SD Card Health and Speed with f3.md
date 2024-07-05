---
created: 2024-07-04T13:05
updated: 2024-07-04T14:18
---
> f3 is a simple tool that tests flash cards capacity and performance to see if they live up to claimed specifications. It fills the device with pseudorandom data and then checks if it returns the same on reading.
> https://fight-flash-fraud.readthedocs.io/en/latest/introduction.html

f3 tests flash storage read/write speed, size and health.

Running f3 on any newly purchased SD card is a good idea, to check that the speed/size match what you were sold, and to confirm there are no issues before you start taking photos!

## Setup

These instructions are for MacOS or Linux.
On Windows, you will need to look into another tool called [h2testw](https://h2testw.org/) (I haven't tested this).

First, we need to install Homebrew, which is a software package manager.

- You can download the [latest release from GitHub](https://github.com/Homebrew/brew/releases) and run the installer
- Or you can paste this command into the Terminal and hit enter:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After Homebrew is installed, we can open the Terminal and install f3:
```
brew install f3
```

## Testing SD cards

You can read more about how to use f3 on their [usage](https://fight-flash-fraud.readthedocs.io/en/latest/usage.html) page.

- Format the SD card that you are looking to test (use your camera to do this?)
- Connect the SD card to your computer
- Check the SD card volume name (open Finder and check the left sidebar)
	- In my case it is called Untitled (that is what my Fujifilm camera formats it to)
	- Volume path will be `/Volumes/SD_Card_Name_Goes_Here`

### f3write

Open the terminal and run f3write using the volume path for your SD card.

```
connor@con-air ~ % f3write /Volumes/Untitled
F3 write 8.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

Free space: 59.44 GB
Creating file 1.h2w ... OK!                         
Creating file 2.h2w ... OK!                         
Creating file 3.h2w ... OK!                         
Creating file 4.h2w ... OK!                         
Creating file 5.h2w ... OK!                         
Creating file 6.h2w ... OK!                          
Creating file 7.h2w ... OK!                          
Creating file 8.h2w ... OK!                          
Creating file 9.h2w ... 14.82% -- 78.62 MB/s -- 12:52

(a few minutes later...)

Creating file 59.h2w ... OK!                        
Creating file 60.h2w ... OK!                       
Free space: 1.00 MB
Average writing speed: 75.05 MB/s
```

### f3read

f3read can now test if all of the data can be read back properly (and will report if any data was corrupted, changed or overwritten).

Run f3read:
```
connor@con-air ~ % f3read /Volumes/Untitled
F3 read 8.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

                  SECTORS      ok/corrupted/changed/overwritten
Validating file 1.h2w ... 2097152/        0/      0/      0
Validating file 2.h2w ... 2097152/        0/      0/      0
Validating file 3.h2w ... 2097152/        0/      0/      0
Validating file 4.h2w ... 2097152/        0/      0/      0
Validating file 5.h2w ... 2097152/        0/      0/      0
Validating file 6.h2w ... 2097152/        0/      0/      0
Validating file 7.h2w ... 2097152/        0/      0/      0
Validating file 8.h2w ... 2097152/        0/      0/      0
Validating file 9.h2w ... 2097152/        0/      0/      0
Validating file 10.h2w ... 2097152/        0/      0/      0
Validating file 11.h2w ... 2097152/        0/      0/      0
Validating file 12.h2w ... 19.69% -- 86.54 MB/s -- 9:22

(a few minutes later...)

Validating file 58.h2w ... 2097152/        0/      0/      0
Validating file 59.h2w ... 2097152/        0/      0/      0
Validating file 60.h2w ...  913475/        0/      0/      0

  Data OK: 59.44 GB (124645443 sectors)
Data LOST: 0.00 Byte (0 sectors)
	       Corrupted: 0.00 Byte (0 sectors)
	Slightly changed: 0.00 Byte (0 sectors)
	     Overwritten: 0.00 Byte (0 sectors)
Average reading speed: 86.88 MB/s
```

Looks like my SD card is still working fine!
If any data is lost, maybe it's time to toss that card... or get a refund.