---
created: 2022-02-10T00:01:00
updated: 2024-07-05T14:26
---
These are notes I took when working on a project to losslessly capture and restore VHS video.  I first captured a number of tapes in 2016 while borrowing a friend's VHS deck.  I didn't end up finishing the project before I had to return the VHS deck to my friend... so 6 years later I ended up paying a pile of $ to buy a VCR of decent quality with a built in TBC (JVC HR-S9911U) on eBay.

If you're curious why the quality of your VCR is so critical to good results when capturing VHS tapes, check out this article: [Video Hardware Suggestions; Best VCRs to Convert Tape to Digital on The Digital FAQ](https://www.digitalfaq.com/guides/video/capture-playback-hardware.htm)

I am using a M1 MacBook Air, using a Windows VM under Parallels.

I link [this video of his](https://www.youtube.com/watch?v=C4PyyQoz6eo) later in the article... but I want to mention up front:  [Andrew Swan](https://www.youtube.com/@Macilatthefront)'s channel was a massive help to getting my VHS restoration workflow set up.  If you are doing anything video related, and want to learn about how to use open source tools like AVISynth, AVISynth+, QTGMC, etc... check out his YouTube channel!
## What I was doing in 2016

List dump of some tools I had installed on Windows when I first started this project back in 2016:
- Neat Video v5 for VirtualDub - https://www.neatvideo.com/download
- WinFF - FFmpeg for windows.
- VirtualDub Filter Pack 2014 - codecpack.co
- Avisynth - script is above.
- DScaler
- HandBrake
- MakeMKV?
- Pinnacle Studio for Dazzle
- Huffyuv

I was running a script like the one below, using AvsPmod on the AVI files that were captured:
```
AVISource("I:\Our Tapes\2000s\12 - 2002good.00.avi")
#ConvertToYUY2()
#Cnr2("xoo",4,2,64) # remove chroma banding noise, wide UV setting
#ChromaShift(C=2, L=-8) # align chroma over luma
#LoadVirtualDubPlugin ("C:\Program Files\VirtualDub\plugins32\ccd2.vdf", "ccd2")
#ConvertToYV12()
AssumeTFF()
QTGMC(Preset="Very Slow", SourceMatch=3, Lossless=2, MatchEnhance=0.75, TR2=1, Sharpness=0.1)
Trim(228,111695)
#SelectEven()
#Stab()
#Tweak(hue=0.0, sat=1.5, bright=-2, cont=1.0, coring=True, sse=false, startHue=0, endHue=360, maxSat=150, minSat=0, interp=16)
#ColorYUV(levels="PC->TV")
Sharpen(0.6)
Spline36Resize(640,480)
Crop(0,0,0,-10)
AddBorders(0,4,0,6)  #Left Top Right Bottom
#ConvertToYUY2()

AVISource("I:\Our Tapes\2000s\2005 shark tooth.00.avi")
#Trim(0,55636)
#ConvertToYUY2()
#Cnr2("xoo",4,2,64) # remove chroma banding noise, wide UV setting
#ChromaShift(C=2, L=-8) # align chroma over luma
#LoadVirtualDubPlugin ("C:\Program Files\VirtualDub\plugins32\ccd2.vdf", "ccd2")
#ConvertToYV12()
AssumeTFF()
QTGMC(Preset="Very Slow", SourceMatch=3, Lossless=2, MatchEnhance=0.75, TR2=1, Sharpness=0.1)
#SelectEven()
#Stab()
#Tweak(hue=0.0, sat=1.5, bright=-2, cont=1.0, coring=True, sse=false, startHue=0, endHue=360, maxSat=150, minSat=0, interp=16)
Sharpen(0.6)
#ColorYUV(levels="PC->TV")
Spline36Resize(640,480)
Crop(0,0,0,-10)
AddBorders(0,4,0,6)  #Left Top Right Bottom
#ConvertToYUY2()
```

## Setting up my capture environment
- I downloaded VirtualDub from here
	- http://www.digitalfaq.com/forum/video-conversion/1727-virtualdub-filters-pre.html
- I downloaded AVSPmod from here
	- http://avspmod.github.io/
- I got AviSynth 260 from here
	- https://sourceforge.net/projects/avisynth2/files/latest/download
- I also saw that there is now AviSynth+ which is a new project continuing development of AviSynth apparently.  That's pretty cool. https://avs-plus.net/
- I downloaded the drivers for my Dazzle video card here:
	- https://archive.org/details/DVC100-drivers
- I downloaded HuffYUV v2.1.1 CCESP Patch v0.2.2 (HuffYUV.rar)
- I downloaded the Lagarith Lossless Codec
	- https://lags.leetcode.net/codec.html

Opened up VirtualDub.  I right clicked VirtualDub.exe and pinned it to the taskbar.

Then I followed this helpful guide on capturing video:
http://www.digitalfaq.com/forum/video-capture/7427-capturing-virtualdub-settings.html

**File menu**
- Set the capture file

**Device menu**
- Uncheck all the options under Device Settings and hit OK
- Pick the Dazzle capture card

**Video menu**
- Select Preview at the top
- Pick the video source (composite or S-Video)
- Under capture pin, use these settings (720x480):
- Under capture filter, check the option for VCR Input
- You can use video levels (brightness/contrast) to fix any clipping
    - review the histogram as you make adjustments
- Compression menu I picked Lagarith Lossless with YUY2 and mutithreading.
- Custom format should have the same settings as picked earlier in capture pin I believe.  resolution 720x480 and color space (YUY2).

**Audio menu**
- Enable audio capture
- Enable volume meter
- Under raw capture format
    - uncompressed PCM: 48000Hz, stereo, 16-bit
- Under compression
    - no compression

**Capture menu**
- Timing
    - DirectShow Options
        - Disable force audio clock
- Enabled timing graph
- Enabled the log window

## Windows 10 didn't work with my capture card
Ran into issues with lag and no audio.  Tried reinstalling drivers I grabbed earlier from archive.org ... no luck.

Found this thread: https://forum.videohelp.com/threads/398965-Dazzle-DVC100-not-capturing-anymore-on-Windows-10 which seemed to have the issue I was experiencing.  Nice.  It's an issue caused by Microsoft.  Well done lol.

The newer driver version on that thread is 1.09, which is a bit newer than the version I got off Archive.org -> Dazzle Video Capture DVC100 X64 Driver 1.07.

Ugh... frig this.  I am just going to install Windows 7 and see how that goes.  I think it'll work better.  Windows 10 is a mess.

## Windows 7 worked fine
Reinstalled Windows 7, installed graphics, capture card, etc drivers, installed Virtualdub pack, Avisynth, Lagarith codec,.  Set up the settings which I detailed above.

Did a test capture and it went GREAT.  Added pic to iCloud Drive. , need to add it here.

## Restoring my videos using AviSynth+

Found this guy Andrew Swan's great tutorials on setting up AviSynth+, wow.  Following that.
- https://www.youtube.com/watch?v=C4PyyQoz6eo
- https://macilatthefront.blogspot.com/2021/01/deinterlacing-with-avisynth-and-qtgmc.html

I did my first captures and was pretty happy with the results.  This is the script I was using for most captures, adapted from my original script (mentioned earlier) I used back when I started this project:

```
AVISource("E:\Baumgaertner-05.avi")
AssumeTFF()   # assumes top frame first interlacing
QTGMC(Preset="Very Fast", SourceMatch=3, Lossless=2, MatchEnhance=0.75, Sharpness=0.1,EdiThreads=4)  # using SourceMatch,Lossless, MatchEnhance settings from the Avisynth QTGMC wiki page.
#Trim(1,280000)
Sharpen(0.3)
Crop(10,0,-4,-7)
Spline36Resize(640,480)
#AddBorders(10,0,0,5)  #Left Top Right Bottom
Cnr2()
Prefetch(10)
```

What I was doing before was cropping and then adding a border.  Now I’m just doing a crop to remove borders from the image and bring the image up to 640x480.

I added Cnr2() for noise reduction.

Then I ended up ordering a JVC Super-VHS VCR deck on eBay because I wanted the image stabilization and the built-in time base corrector.

I got the SVHS VCR set up and used my old VCR deck just to handle tape rewinding.

~ fin ~

(might post some samples here if I feel like it someday)