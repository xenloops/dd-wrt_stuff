# Peacock Thread -- _for Broadcom only_

My distillation of the essential [Peacock Thread](https://forum.dd-wrt.com/phpBB2/viewtopic.php?t=51486) by Murrkf for Broadcom router chipsets. Why? The original, useful as it is, is a meandering path of informative nuggets wrapped in a cesspool of needless verbiage. If you want a Hemingway novel, walk to your friendly neighborhood indie bookshop or library instead.

Read through all the notes starting with [Note 1](#note1), or just pick and choose below.

## Must read notes

  * Note 1: Hard Reset and DD-WRT upgrade info [Note 1](#note1)
  * Note 2: Backup files can't be reused [Note 2](#note2)
  * Note 3: Recommended builds [Note 3](#note3)
  * Note 4: Understanding DD-WRT build options [Note 4](#note4)
  * Note 5: Always follow the wiki for first time flashing [Note 5](#note5)
  * Note 7: Required Posting Info [Note 7](#note7)
  * Note 9: How to backup your CFE - DO IT! [Note 9](#note9)
  * Note 18: Miscellaneous EXTREMELY Useful Info [Note 18](#note18)

## Notes for Specific Issues
  
  * Test for bricked routers [Note 6](#note1)
  * Don't Pin Short! [Note 8](#note8)
  * Memory Issues/P2P [Note 10](#note10)
  * How to Tftp [Note 11](#note11)
  * Limiting Bandwidth [Note 12](#note12)
  * Blank Webgui/White Page [Note 13](#note13)
  * Modem/Wan IP Issues [Note 14](#note14)
  * Bridging Routers/Public Wifi [Note 15](#note15)
  * Supported Routers [Note 16](#note16)
  * Power Supply/Hardware Issues [Note 17](#note17)

<details>
  <summary> Note 1: Hard Reset and DD-WRT upgrade info <a name="note1"></a> </summary>
  
  ## Hard resets
  
  [Hard reset / 30-30-30 reset page](https://forum.dd-wrt.com/wiki/index.php/Hard_reset_or_30/30/30)
  
  Hard resets will not remove dd-wrt from your router. Hard resets usually do not work with stock firmware.
  
  Router-specific notes:
  * Linksys EA Series: **do not do this process**. Use the factory reset option instead. It has NVRAM storage that holds important information which cannot be erased. If the router has already been through a hard reset, see [Robb's instructions on how to fix it](http://www.dd-wrt.com/phpBB2/viewtopic.php?p=920100#920100).
  * Linksys WRT54GS [v1.1](http://dd-wrt.com/wiki/index.php/Linksys_WRT54GS_v1.1), [v2.0](http://dd-wrt.com/wiki/index.php/Linksys_WRT54GS_v2.0), and [v2.1](http://dd-wrt.com/wiki/index.php/Linksys_WRT54GS_v2.1) models can brick after a hard reset. See [this thread and the solution in Vulcan's post](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=45024).
  * [Linksys WRT320N](https://wiki.dd-wrt.com/wiki/index.php/Linksys_WRT320N_v1.0) has a faulty reset button. See [this post about using the WPS button to erase nvram](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=63004).
  * [Asus RT-N16](https://wiki.dd-wrt.com/wiki/index.php/Asus_RT-N16): the reset button puts it into firmware restore mode. See the [RT-N16 wiki](https://wiki.dd-wrt.com/wiki/index.php/Asus_RT-N16#Install_DD-WRT_from_Factory_Firmware) for how to reset this router.
  * Otherwise, perform a hard reset _before_ and _after_ changing DD-WRT versions.

  After doing a hard reset after DD-WRT is installed, if the newly DD-WRTized router doesn't force you to enter an initial password when you try to login to the router at 192.168.1.1 (for DD-WRT builds newer than 9707 from June 14, 2008), you haven't done the hard reset properly. Failing to do a hard reset properly and waiting after flashing are the two most common errors that lead to unnecessary DD-WRT pain. This step clears information the original router has written to the NVRAM. If any of the old information is present with the new DD-WRT firmware, it may not operate properly. Don't cut corners. Doing it before you upgrade can be very important; a hard reset is not just for after upgrades.
  
  ## Upgrading an existing DD-WRT installation
  
  Once DD-WRT is installed, follow these general steps for upgrading DD-WRT:
  
  1. Set your computer to a unique static IP on the subnet the router is on. Disable all firewalls and security. Disable wireless on your computer and connect the router connected _only_ to the computer flashing the firmware by an Ethernet cable.
  1. Perform a hard reset. Wait. Check for the new password page and change the password.
  1. Use the DD-WRT upgrade page to flash the firmware, unless it is a Belkin router (use tftp.exe to flash Belkins).
  1. Wait _at least three minutes_. Lights should return to normal. Impatience is how many routers get bricked.
    * After flashing firmware but _before_ the hard reset, the router is writing some NVRAM settings. **_Wait for this process to finish before doing anything, including a hard reset._** Usually the WLAN light illuminates when this is finished, and it can take several minutes. Have a frosty beverage of your choice. Some prefer beer, some wine, some whiskey. Many abstain, but frankly if you're putzing with flashing router firmware you probably need to relax a little at this point. Go outside, appreciate the world.
  1. Power cycle the router (unplug the cord, count to 30, and plug it back in). 
    * Some routers need special handling (e.g. the Asus RT-N16 30/30/30 reset method uses the WPS button instead of the Restore button -- see your router model's DD-WRT wiki page).
  1. Wait for the lights to return to normal, usually about 2 minutes.
  1. Perform a hard reset again. Wait. Check for the password page and set the password. 
  1. Finally reconfigure your settings manually.
  1. Once configured set your computer back to automatic IP and DNS. 
  
  If you have flashed a correct build (see Note 4) the proper way and the router looks like it is running the same firmware as before, clear the browser cache.
  
</details>

<details>
  <summary> Note 2: Restoring configuration backups after changing builds can brick your router
 <a name="note2"></a> </summary>

  * Do not try to upload old configuration files from one build on another version.
  * Delete your old configuration files once you are sure the newer firmware is stable. They are useless.
  * Do not use backup configuration files from one router model on another router model.
  * Do not use old config files _if_ you are having any problems; you could reintroduce the problem. 

  When upgrading, you do have to re-enter your settings again. The benefit to this is that you might see new options, or options you missed earlier. Try Imacros for Firefox to automate the process if needed, or read about [some scripts that can be used to restore some parts of the nvram](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=44324).
  
</details>

<details>
  <summary> Note 3: Recommended builds <a name="note3"></a> </summary>

  * Use a build recommended in this Note (especially if running SP1 or v24 final (05/21/08) 13064 or 14896) or the router specific thread for your router for best  stability.
  * All of these forum recommended builds are **beta versions**, and not "finished" yet. Use them at your own risk, though the forum-recommended builds have been thoroughly tested. Browse the forums to see what others say about the builds.
  * Builds newer than the recommended builds are **alpha versions** and have been released for testing only. Some of the latest 25xxx buids seem to be reasonably good, but some builds can have problems. If installing a different build than the recommended builds, assume you are testing and you might find that it does not work. Each build has a build thread in the forum created when the build is released. Report problems in that thread, but do not ask for help with your router in the build thread. The only exception to this is if you are using a very new router which requires initial flashing of a build that is newer than 15962. In that case most should use 17990 or 18000. For newer routers check the supported devices wiki for the version first to be supported on your router.
  * Never flash a build with a _lower_ number than the first supported build listed in the router's wiki page. Your router must first be flashed with a build _later_ in number than the build that was first used on the router by DD-WRT.
  * Do not rely on the build listed in the router database. The router database has recommended some less stable builds (e.g. 13064 (10/10/09) or 14896). Use the builds recommended here instead. Sometimes the router database also has had the wrong build type. The router database is being worked on improve the recommendations but it still contains errors and bad builds. 
  * Do not skip reading the wiki for your router. It may include special instructions for installing on that router. The wiki has the most up-to-date information for a router.

### Following are the recommended builds
 
  * **Brainslayer 14929** works fine in most configurations and routers and should be used in most cases. To try a newer build due to having a newer router that requires it, Build 15962 has good stabilty and is a good overall build for the e2000 and e3000 routers.
    * Recommended build downloads:
      * K2.4 builds: ftp://ftp.dd-wrt.com/betas/2010/08-12-10-r14929/broadcom/
      * K2.6 builds: ftp://ftp.dd-wrt.com/betas/2010/08-12-10-r14929/broadcom_K26/ 
        * Use K2.6 _only_ if required by the model router (usually newer ones). Some newly supported routers can _only_ use K2.6 builds. Routers that originally needed a build with K2.6 in the name will likely always require a K2.6 build.
        * If your router can use both K2.4 and K2.6, use K2.4.
        * Putting K2.6 on a router that can only use K2.4 _will_ brick it.
        * Do _not_ use builds after 15314 K2.6 with CPU 4704 (corerev=11). 
    * There have been major problems with some builds newer than 15962.
    * If using a newer router model (e.g. e4200) that can only run more recent builds than 15962 and you don't need SSH, use Build 17990, 18000, 25760, or 27858. 
    * 12548 is a good build for older G only routers.
  * Check the NVRAM size of the router for newer models (e.g. e2000, e3000, e4200), as they require an nv60 or nv64 to be flashed. 
  * If having problems using 14929 (especially if Repeater Mode doesn't work), try downgrading to EKO 12548 if your router supports that build.
  * For VINT support (for very old routers, see Note 4 on this), Build 13491 is a recommended VINT build. 12548 also worked well for many.
  * WPA2-AES is secure and working well in these builds. No other encryption except WEP works reliably with DD-WRT in these builds, and WEP has been broken as a security protocol since the 2000s -- so don't use it.
 
</details>

<details>
  <summary> Note 4: Understanding DD-WRT build options <a name="note4"></a> </summary>
 
 CHECK and UNDERSTAND the proper build for your router. The ROUTER memory (flash memory...not RAM) determines what build you can put on it. It is simple math. If you have a 4mb flash chip, you can't put a build larger than 4mb on it because it won't fit, even if you wish to have advanced features. In other words, don't look at the features and decide which one you want and then install it. You will often brick your router. This is bad.


Trailed builds are builds with the router model in the name. BS builds are often, but not always trailed builds. These builds are often necessary for initial flashing of a virgin router because they have the right header in the file that allows it to be flashed on the stock firmware. Once dd-wrt is installed, you should not use a trailed build, and should use a generic build instead UNLESS you have one of the new Linksys Routers (E2000, E3000 E4200) that allocate nvram differently. Those routers (eg. E2000 and E3000, E4200) must ALWAY USE A TRAILED BUILD for initial flashing, and some upgrades from initial flashing REQUIRE nv60 or nv64 builds. (See below for further info.).

Additional Note for E2000 E3000 and E4200 etc. users:
Do the initial flash with the trailed build that contains your router model number...(example...xxx_e2000.bin for a E2000 model) Once you have done the initial flash...you can upgrade firmware using the build that contains xxx_e2k_e3k.bin for all subsequent flashes. (If you are using a 16758 or higher build, which is newer than is recommended, you must use a nv60k build rather than the e2k-e3k build - See the wiki for your router and this post: http://www.dd-wrt.com/phpBB2/viewtopic.php?t=148350)

Builds with nv60k in the name can only be used with routers that have a 60k nvram space. Builds with nv64k in the name can only be used on routers that have a 64k nvram space. If you use one of these builds and your router doesn't support it, you will brick your router and will need a serial cable to fix it. If you don't use one of these builds and your router needs it, you will brick your router and will need a serial cable to fix it. These nv60k/nv64k are only being used in many new routers, and the need to use these builds, if it applies to your router SHOULD be explained in the wiki for your router. No k24 supported routers use nv60/nv64.

See this thread for specific models that must use nv60 or nv64 builds:
http://www.dd-wrt.com/phpBB2/viewtopic.php?t=156851

FOLLOW THE PROCESS FOR FLASHING YOUR ROUTER THAT IS IN THE WIKI.
http://www.dd-wrt.com/wiki/index.php/Installation
Once you have dd-wrt installed on your router, you can change to any generic stable build that the flash will support by upgrading using the webgui. You should not have to stick with the build in the wiki install for your router, but you do need to understand what builds can be flashed on your router!

Read the rest of this note to understand builds to flash to your router, once dd-wrt is installed according to the wiki, and to make sure you are flashing a supported build for your router.

To pick a build, there are three things you need to know. The process for flashing, the build type (micro, mini, standard or mega) and whether you need newd, K26, nv60 or nv64. Then you can pick the build svn (14929, 17990, 12548 etc.) that you want to flash.

You don't have to use the build VERSION that are recommended in the wiki but it is a good practice to do so unless you undestand how all this works. You should use the build TYPE. (See below in this note.)

You need to know whether you can put micro, mini, standard or mega on your router. You can determine what TYPE of build is supported on your router by going here:
http://www.dd-wrt.com/wiki/index.php/Supported_Devices

The supported devices wiki will also tell you the FLASH size that your router has. (DON'T Confuse flash with ram size). Your router will have 2, 4 or 8mb (or higher with modern routers)of Flash. The generic builds are not based on router models; trailed builds are only used for initial flashing. (Except for non standard nvram like E2000, E3000 Linksys)

To determine what version TYPE to flash, use this guide:

If you have 2mb of flash, you MUST use Micro.bin. (See below for how to determine if you need NEWD_Micro.bin or VINT_Micro.bin). You cannot use micro PLUS on ANY router unless it has a compressed CFE. If you didn't compress your CFE, or don't know what a compressed CFE is, DON'T USE MICRO PLUS. Info on compressed CFEs can be found here:
http://www.dd-wrt.com/phpBB2/viewtopic.php?t=38844

If you have 4mb of flash, you can use any mini or standard build as long as you get the newd/k26/nv60/nv64 choice right (see below in this note). You cannot use Mega on a router with 4mb of flash. Some of these builds have different features built in. If you don't need VPN, then you can get a VPN version, for example.

IF you have 4mb of flash, and you don't know what build to use, just use MINI.bin. You can always reflash later.

If you have 8mb of flash, you can use any generic build you wish. (Subject to k26 or nv60/64 specific builds that your specific router might need), unless the build is larger than the 8mb flash size. (Some recent mega builds have been) .
Maximum firmware size
To be on a safe side, please check the size of new firmware file before flashing it to your router. Flashing too large firmware file can brick your router. (Netgear Routers often cannot flash standard builds because of the extra partition they have on the flash chip.)
2MB flash chip / normal cfe (256k) : 1769472 bytes
2MB flash chip / compressed cfe (128k): 1900544 bytes
4MB flash chip (not Netgear): 3801088 bytes
4MB flash chip Netgear routers: 3735552 bytes
8MB flash chip: 7995392 bytes

Micro builds are not recommended for 8mb flash routers and Do NOT put a micro build on a router with an N radio. Using micro on an N router can brick some N routers.

NEWD vs VINT vs NEWD2 vs K26

NEWD is the NEW Driver based on the kernel 2.4. Many older routers use this. Vint is the very old Vintage driver for really early Linksys router from about 15 years ago, also based on kernel 2.4. Newd-2 is a newer driver that is available both in a kernel 2.4 and a kernel 2.6. Don’t use builds with newd2 in the name. K26 is the new kernel that some very new routers MUST use and it has the newd-2 driver. Using K26 on a router that doesn't support it, or failing to use K26 on a router that requires it will both brick the router.

NEWD has only NEWD (NOT NEWD2) in the filename of the bin file...DON'T CONFUSE THIS WITH NEWD2. All Brainslayer builds without k26 in the filename are NEWD builds. NEWD-2 has NEWD2 in the filename. K26 has K26 in the filename of the .bin file. VINT has the word VINT in the filename.

Most routers should use NEWD rather than VINT when they can. VINT is for old routers that cannot support the new wireless drivers and have a correv of 4 or less. Many new models can ONLY use k26 builds, as explained above - if your original flash from the wiki was a build with k26 in the filename, you must always flash with a k26 build.
Correvs for many models are listed in the wiki:
http://www.dd-wrt.com/wiki/index.php/Corerev
If you have dd-wrt installed, here the command that you send to the router to check:

nvram get wl0_corerev

Do this from a telnet session or putty terminal window.
It can also be done from the Administration>>Commands tab.
In addition, some routers use a 60 or 64k nvram space, and must use EITHER a 60 or 64K build.
CHECK all of this (Nv60, Build allowed etc.)and be sure. However, some of the information in the Wiki is out of date in that there are sometimes newer builds but generally the wiki install for your router has reliable specific instructions. If it says you can use generic_standard, then any standard build can be used.

So, if it says that you can use, for example Generic_Standard v.24 12548 you could also use NEWD_Standard svn12874, or, if you need a VINT build, VINT_Standard svn12548. You should never use builds that have a specific name of the router in the name of build that is not the name of YOUR router! (These are also called 'trailed builds') Ie. Don't use v24-10700_NEWD_mini_wr850g-v2-v3.bin (which is for a wr850 v.2 router, in a WRT54gl router! If there is not a specific build for your router, use flash size to determine the build, as explained above.

HOWEVER, especially with a newer router, please note that you can never flash a router with a build that predates the support for that router. Some very new router have just been supported (e2500, e3200 etc.). You cannot flash any build that was created before support was added for that router. So with newer router, you need to search to find out what build was the first one that could be used on that router, and make sure you ONLY FLASH builds that are that build or newer. Using 14929 will brick a newer router model that was not supported when 14929 was released.

If you want to know the differences between the versions, and what each of the different builds includes, see this page in the wiki:
https://wiki.dd-wrt.com/wiki/index.php/What_is_DD-WRT%3F#File_Versions
For example, if you need a VPN version, you can check which build types support it, but remember that ALL other considerations (n64, version supported, k26 etc) STILL have to be correct for you to flash the build type you want. (DHC_Darkshadows has done some good work to keep that page detailed and up to date.}

NEWD-2 was developed for some routers that could also use NEWD. So there was a choice of NEWD, or NEWD-2 for these routers. When k26 was developed it also used NEWD-2. So this meant there was a k24NEWD, and K24 NEWD-2 and and K26 NEWD-2. ALL k26 builds are NEWD-2.

Don't use a kernel24 newd-2. There is no benefit that we have found. However, most routers developed in the last two years can ONLY use K26. Make sure you see this thread and check if your router can use K26 before flashing:

http://www.dd-wrt.com/phpBB2/viewtopic.php?t=63757&start=0

Also, to tell if you have one of the new routers that MUST ONLY use K26, you simply check the wiki install for your router and if you originally were told to use a K26 or a NV60 or NV64 build, you must continue to ALWAYS use a k26/nv60/nv64 build.

IF you are wondering if you should use a build with olsrd in the name, then the answer is no. This is a case of "if you don't know what it is, you don't need it". OLSRD is for mesh networks.

IF you have looked, and are not sure, ASK.

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

