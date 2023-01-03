# Peacock Thread

My distillation of the famous [Peacock Thread](https://forum.dd-wrt.com/phpBB2/viewtopic.php?t=51486) by Murrkf for Broadcom router chipsets. Why? The original, useful as it is, is a meandering path of informative nuggets trapped in a cesspool of needless verbiage. If you want a Hemingway novel, walk to your friendly neighborhood indie bookshop instead.

Read through all the notes starting with [Note 1](#note1), or just pick and choose below.

*For Broadcom only!*

## Must read notes

  * Note 1: Hard Reset and DD-WRT upgrade info [Note 1](#note1)
  * Note 2: Backup files can't be reused [Note 2](#note2)
  * Note 3: Recommended builds [Note 3](#note3)
  * Note 4: Understanding dd-wrt build options [Note 4](#note4)
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

  When upgrading, you do have to re-enter your settings again. You might see new options, or some options you missed earlier. Try Imacros for Firefox to automate the process if needed, or read about [some scripts that can be used to restore some parts of the nvram](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=44324).
  
</details>

<details>
  <summary> Note 3: <a name="note3"></a> </summary>

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

<details>
  <summary> Note : <a name="note"></a> </summary>

</details>

