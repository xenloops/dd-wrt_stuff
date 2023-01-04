# Peacock Thread -- _for Broadcom-based routers only_

My distillation of the essential [Peacock Thread](https://forum.dd-wrt.com/phpBB2/viewtopic.php?t=51486) by Murrkf for Broadcom router chipsets. Why? The original, useful as it is, is a meandering path of informative nuggets wrapped in a cesspool of needless verbiage. If you want a Hemingway novel, walk to your friendly neighborhood indie bookshop or library instead.

Read through all the notes starting with [Note 1](#note1), or just pick and choose below.

## Must read notes

  * Hard Reset and DD-WRT upgrade info [Note 1](#note1)
  * Backup files can't be reused [Note 2](#note2)
  * Recommended builds [Note 3](#note3)
  * Understanding DD-WRT build options [Note 4](#note4)
  * Always follow the wiki for first time flashing [Note 5](#note5)
  * Required posting info [Note 7](#note7)
  * How to backup your CFE - DO IT! [Note 9](#note9)
  * Miscellaneous extremely useful info [Note 18](#note18)

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
 
  Various factors govern which builds are right for a router. It is the user's responsibility to double-check and understand the proper build to use. 
 
  * The router's _flash_ memory (not RAM) determines which builds can run on it. A router with a 4 MB flash chip simply cannot take a build larger than 4 MB, no matter how tantalizing the advanced features. _Attempting to install a larger build than will fit on the router will often brick the router._
  * Trailed builds are builds with the router model in the name. "Brainslayer" builds are often (but not always) trailed builds. These builds are often necessary for initial flashing of a router with OEM firmware because they have the right header in the file that allows it to be flashed onto the stock firmware. Once DD-WRT is installed, do _not_ use a trailed build, but install a generic build instead.
  * Certain Linksys routers (e.g. E2000, E3000, and E4200) that allocate NVRAM in a nonstandard way are the exception. These routers must always use a trailed build for initial flashing, and some upgrades from initial flashing require the nv60 or nv64 builds. For these routers:
     * Do the initial flash with the trailed build that contains your router model number (e.g. xxx_e2000.bin for an E2000 model). Once the initial flash is done, upgrade using the build that contains xxx_e2k_e3k.bin for all subsequent flashes. (
     * If using a 16758 or higher build, you _must_ use a nv60k build rather than the e2k-e3k build - See the wiki for your router and [this post](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=148350).
     * Builds with "nv60k" in the name can only be used with routers that have a 60k NVRAM space, and those with "nv64k" in the name can only be used on routers that have a 64k NVRAM space. Using one of these builds on a router that doesn't support it _will brick the router_. Not using one of these builds on a router that needs it _will also brick the router_. The router's wiki page shows whether the router needs to use these builds. No k24 supported routers use nv60/nv64. See [this thread](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=156851) for specific models that must use the nv60 or nv64 builds.
 
 ## Flashing the router
 
  * Follow the process in the [Installation wiki page](http://www.dd-wrt.com/wiki/index.php/Installation) to flash the router. 
  * Once DD-WRT is installed on the router, you can change to **any generic stable build** that the flash will support by upgrading using the DD-WRT GUI. You do not have to stick with the build in the wiki install for your router, but do understand what builds can be flashed to your router.
  * To choose a build, understand these factors:
    1. The process for flashing
    1. The flash memory size of the router (not to be confused with RAM). See the [Supported Devices](http://www.dd-wrt.com/wiki/index.php/Supported_Devices) wiki.
    1. The build type (micro, mini, standard, or mega). You should use the build type specified in the router's wiki page and/or [Supported Devices](http://www.dd-wrt.com/wiki/index.php/Supported_Devices) wiki.
    1. Whether you need newd, K26, nv60 or nv64.
    1. Then you can pick the build (14929, 17990, 12548 etc.) that you want to flash. You don't _have_ to use the build version recommended in the wiki, but it is a good practice to do so unless you undestand how all this works.

Use this table to determine which type to flash (also see notes below table):

| Type options | Micro | Mini | Standard | Mega |
|--|--|--|--|--|
| Min router flash size | 2 MB | 4 MB | 4 MB | 8 MB |
| Notes | NEWD/Vint | NEWD/k26/nv60/nv64 | NEWD/k26/nv60/nv64 | Builds up to 8 MB |

Check the size of new firmware file before flashing it to the router. Flashing too large a file can brick the router.

| Flash size (MB) | Max firmware size (b) |
|--|--|
| 2 | 1,769,472 (1,900,544 compressed CFE) |
| 4 | 3,801,088 (3,735,552 Netgear) |
| 8 | 7,995,392 |

Notes:
  * Do _not_ use Micro builds on routers with N WiFi. This can brick the router.
  * Do _not_ use Micro Plus on any router unless it has a [compressed CFE](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=38844). If you didn't compress your CFE, or don't know what it is, _do not use Micro Plus_.
  * Netgear routers often cannot flash standard builds due to an extra partition on the flash chip.
  * For routers with 8 MB flash, use any generic build you wish (subject to k26 or nv60/64 specific builds that your specific router might need), unless the build is larger than the 8 MB flash size, as with some later Mega builds.
  * For routers with 4 MB flash, if you don't know what build to use, flash Mini. You can always reflash later.
  * Do _not_ use Micro builds for 8+ MB routers.
  * NEWD vs Vint vs NEWD2 vs K26: 
    * NEWD is the **new d**river based on Linux kernel 2.4. Many older routers use this. NEWD has only "NEWD" (not "NEWD2") in the filename. All Brainslayer builds without "K26" in the filename are NEWD builds. Most routers should use NEWD rather than Vint when they can.
    * Vint is the older **vint**age driver for early Linksys routers, based on kernel 2.4. Vint has "VINT" in the filename. Vint is for old routers that cannot support the new wireless drivers.
    * NEWD-2 is a newer driver that is available both in a kernel 2.4 and a kernel 2.6. NEWD-2 has "NEWD2" in the filename. Don't use a kernel24 NEWD-2. There is no benefit that we have found.
    * K26 is the kernel that some newer routers _must_ use, containing the NEWD-2 driver. If the original flash specified in the router wiki was a build with "k26" in the filename, that router must _always_ use a k26 build. Using K26 on a router that doesn't support it, or failing to use K26 on a router that requires it, _will brick the router_. K26 has "K26" in the filename. [This thread](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=63757&start=0) shows whether many routers need K26.
    * Some routers use a 60 kb or 64 kb NVRAM space, and must use either a 60K or 64K build.
    * Never use a build that predates the support for that router.
    * Never use builds that have a specific name of a router in the name of build unless it is the name of your router (these are called 'trailed builds').

To find the core revision of the router (if needed):
   * Corerevs for many models are listed in the [Corerev wiki](http://www.dd-wrt.com/wiki/index.php/Corerev).
   * If the router already has DD-WRT installed, enter one of these commands in the Administration > Commands tab (or using a Telnet session or PuTTY terminal):
   ```nvram get wl0_corerev```
   or
   ```nvram show|grep corerev```

Example: If the router's wiki page says that you can use Generic_Standard v.24 12548, you could also use NEWD_Standard svn12874, or, if you need a VINT build, VINT_Standard svn12548. 
 
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

