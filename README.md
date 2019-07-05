# Phantom installation in Linux

>coming soon...

## Compatibility Test

See https://github.com/jaejunlee0538/sensable_phantom_in_linux/wiki/Compatibility-Test-Log 

## Installation

##### Download PDD and OpenHaptics

Download PDD (Phantom Device Driver) and Open Haptics from ~~[Sensable DSC][] homepage~~ the link is now broken, you can find the same [zip file here][zip_link].

In this tutorial, we used the following versions.
* OpenHapticsAE Linux v3.0
* Phantom Device Driver 4.3

And also download [Sensable_Phantom_libs.tar.gz][].

`Sensable_Phantom_libs.tar.gz` contains library files required for Ubuntu versions higher than 11.04, which are not compatible with PDD in terms of raw1394 package.

----------------------------------------------------------------------
##### Installing PDD
Extract files from downloaded `OpenHapticsAE_Linux_v3_0.zip` file.

>**x86**
```sh
$ cd [DOWNLOAD_DIR]/OpenHapticsAE_Linux_v3_0/OpenHapticsAE_Linux_v3_0/PHANTOM Device Drivers/32-bit
$ sudo dpkg -i phantomdevicedrivers_4.3-3_i386.deb
```

>**x64**
```sh
$ cd [DOWNLOAD_DIR]/OpenHapticsAE_Linux_v3_0/OpenHapticsAE_Linux_v3_0/PHANTOM Device Drivers/64-bit
$ sudo dpkg -i phantomdevicedrivers_4.3-3_amd64.deb
```


Install required packages for PDD.
```shell
$ sudo apt-get install freeglut3-dev x11proto-gl-dev libmotif-dev mesa-utils libglw1-mesa-dev
```

>If you encounter any problem with `libGLw*` file, you have to read [DSC_forum_libGLw][].
>
>`libglw1-mesa-dev` was installed for getting `libGLw*` files.
>But originally, downloading `libGLw.so, libGLw.so.1, libGLw.so.1.0.0` files and copying it to `/usr/lib` directory was suggested (see [here][Phantom Omni Package Installation on ROS Fuerte & ROS Groovy]). Instead, we tried installing `libglw1-mesa-dev`  because it seems to be compiled with motifs support as required by PDD, and it worked. If you encounter any problem with this library, use the included library files in `libGLw.tar.gz` instead (32 bits only).

Extract files from `Sensable_Phantom_libs.tar.gz` and replace libPHANToMIO.so.4.3 file with included in Linux_JUJU_PDD_XX-bit folder.

>Originally JUJU_PDD file was uploaded in [DSC_forum_JUJU_PDD][], but download link is not available now.

Run the following commands.

>**x86**
```shell
$ cd [DOWNLOAD_DIR]
$ mkdir Sensable_Phantom_libs && tar -xvf Sensable_Phantom_libs.tar.gz -C Sensable_Phantom_libs
$ cd Sensable_Phantom_libs/Linux_JUJU_PDD_32-bit
$ sudo cp PHANToMConfiguration /usr/sbin
$ sudo cp libPHANToMIO.so.4.3 /usr/lib
```

>**x64**
```shell
$ cd [DOWNLOAD_DIR]
$ mkdir Sensable_Phantom_libs && tar -xvf Sensable_Phantom_libs.tar.gz -C Sensable_Phantom_libs
$ cd Sensable_Phantom_libs/Linux_JUJU_PDD_64-bit
$ sudo cp PHANToMConfiguration /usr/sbin
$ sudo cp libPHANToMIO.so.4.3 /usr/lib
```

If old symbolic links exist, you have to remove them.
To check it, run this command:

```shell
$ find /usr/lib -name "libPHAN*"
```
Two possible scenarios from here:

- ###### Case 1 : three results

  ```shell
  $ find /usr/lib -name "libPHAN*"
  /usr/lib/libPHANToMIO.so.4.3
  /usr/lib/libPHANToMIO.so.4
  /usr/lib/libPHANToMIO.so
  ```

  If you have the output same above, you have to delete broken symbolic links, because
  `libPHANToMIO.so.4` and `libPHANToMIO.so` had symbolic links to old `libPHANToMIO.so.4.3` file.

  ```shell
  $ cd /usr/lib
  $ sudo rm libPHANToMIO.so
  $ sudo rm libPHANToMIO.so.4
  ```

  Then, you have to create the missing symbolic links with the same name.

  ```shell
  $ sudo ln -s libPHANToMIO.so.4.3 libPHANToMIO.so
  $ sudo ln -s libPHANToMIO.so.4.3 libPHANToMIO.so.4
  ```

- ###### Case 2 : one result

  ```shell
  $ find /usr/lib -name "libPHAN*"
  /usr/lib/libPHANToMIO.so.4.3
  ```

  If you have the output same above, you have to add the missing symbolic links.

  ```shell
  $ cd /usr/lib
  $ sudo ln -s libPHANToMIO.so.4.3 libPHANToMIO.so
  $ sudo ln -s libPHANToMIO.so.4.3 libPHANToMIO.so.4
  ```



## Run PHANToMConfiguration and PHANToMTest

In this chapter, we will run PHANToMConfiguration program to configure the devices and run PHANToMTest to check the device is working well. Firstly, we are going to use **Phantom Omni** (IEEE-1394 interface) and after then try with **Phantom Premium 1.5** (Parallel port)  

First, let's run PHANToMConfiguration program.

If you are using Ubuntu version of 11.04 or higher, you might see this error message.
```Shell
$ /usr/sbin/PHANToMConfiguration
/usr/sbin/PHANToMConfiguration: error while loading shared libraries: libraw1394.so.8: cannot open shared object file: No such file or directory
```

> In x64 Ubuntu, PHANToMConfiguration worked well without the above problem.
> But when running PHANToMTest, you will encounter `libraw1394.so.8` error.
> So anyway you have to run the following command.


To solve this, create symbolic link for libraw1394.so.8.

>**x86**
```shell
$ cd /usr/lib/i386-linux-gnu
$ sudo ln -s libraw1394.so.11 libraw1394.so.8
```

>**x64**
```shell
$ cd /usr/lib/x86_64-linux-gnu
$ sudo ln -s libraw1394.so.11 libraw1394.so.8
```


Then try to run PHANToMConfiguration again.
```shell
$ /usr/sbin/PHANToMConfiguration
```

![image_omni_configuration][]

Yet one error message showed up, Phantom omni was working fine.

```shell
$ /usr/sbin/PHANToMConfiguration
Warning: Cannot convert string "-adobe-helvetica-medium-r-normal--14-100-100-100-p-76-iso8859-1" to type FontStruct
Warning: Cannot convert string "-adobe-helvetica-bold-r-normal--14-100-100-100-p-82-iso8859-1" to type FontStruct
PDD Error: raw1394 module not loaded, modprobe raw1394 to correct
```

Let's run PHANToMTest program (If you have a GLX issue, just see below).

```shell
$ /usr/sbin/PHANToMTest
Warning: Cannot convert string "-adobe-helvetica-medium-r-normal--14-100-100-100-p-76-iso8859-1" to type FontStruct
open_load_connection: open(PIPE_NAME,O_WRONLY,O_NDELAY): : No such file or directory
```
![image_omni_test][]

 

> **If PHANToMTest don't start** and you have this kind of error with GLx instead :
>
> ```shell
> $ /usr/sbin/PHANToMTest
> Warning: Cannot convert string "-adobe-helvetica-medium-r-normal--14-100-100-100-p-76-iso8859-1" to type FontStruct
> X Error of failed request:  BadValue (integer parameter out of range for operation)
>   Major opcode of failed request:  155 (GLX)
>   Minor opcode of failed request:  3 (X_GLXCreateContext)
>   Value in failed request:  0x0
>   Serial number of failed request:  585
>   Current serial number in output stream:  586
> ```
>
> It is because Indirect GLX context is not enabled in your Xorg configuration, and it is required for PHANToMTest program to run.
>
> First, run this command :
>
> ```
> $ locate xorg.conf
> ```
>
> You will have something like this :
>
> ![locate_xorg_conf][]
>
> Since recent Xorg server program, there is not an unique`xorg.conf` file anymore, but multiple.
>
> To create a new config file for your phantom device in  `/usr/share/X11/xorg.conf.d/` , you can just copy the file `phantom.conf` from the git repo into the folder,  or directly create one, for example with the name `phantom.conf`:
>
> ```
>$ sudo gedit /usr/share/X11/xorg.conf.d/phantom.conf
> ```
> 
> And add these lines :
>
> ```
>Section "ServerFlags"
> 	Option "AllowIndirectGLX" "on"
> 	Option "IndirectGLX" "on"
> EndSection
> ```
> 
> Save the file, and **reboot** the computer to restart Xorg server with the new configuration.
>
> Try to run again PHANToMTest program, it should work now:
>
> ```
>$ /usr/sbin/PHANToMTest
> ```
> 
> More info about the Indirect GLX Issue [here][LINK_GLX_ISSUE].



**Back to the running PHANToM Test program.**

Click space-bar to move to the next step. Encoders, force feedback, 2 buttons were fine.

Box test was also done with no problem.

![image_omni_box_test][]

If you want to test with **Phantom Premium 1.5 (parallel port)** also, keep reading this chapter.

>**You must read this portion if you have to use parallel port for Phantom.**

>Built-in parallel port was used for this test.

Run the following commands.
>**x86**
```shell
$ cd /usr/lib/i386-linux-gnu
$ sudo ln -s libraw1394.so.11 libraw1394.so.8
$ /usr/sbin/PHANToMConfiguration
```

>**x64**
```shell
$ cd /usr/lib/x86_64-linux-gnu
$ sudo ln -s libraw1394.so.11 libraw1394.so.8
$ /usr/sbin/PHANToMConfiguration
```


Manipulate configuration window for your device.

Test device was Phantom Premium 1.5A equipped with high-resolution gimbal.

![image_premium_1.5_configuration][]

**Before running PHANToMTest**, you have to finish additional jobs.

First, you have to check your parallel port is running on EPP mode.

>EPP mode communication is required to support this high-speed channel.
>--[PDD and OH Installation Guide For Linux][]--

To check EPP mode, execute the following command.
In this tutorial, our parport was assigned to `parport0`.

``` shell
$ cat /proc/sys/dev/parport/parport0/modes
PCSPP,TRISTATE,EPP
```

If you find ECP instead of EPP, then go to the BIOS setting and change the parallel port mode to EPP before going to the next step.

![image_bios_epp_setup][]



**At this stage**, you should launch this command to check the configuration of the parallel port :

```shell
$ dmesg | grep parp
```

This will give you the boot-log of the parallel port. Two possible scenarios from here:

- ###### Case 1 : 

  ```shell
  $ dmesg | grep parp
  [    8.430242] parport0: PC-style at 0x378, irq 7 [PCSPP,TRISTATE,EPP]
  [    8.524323] lp0: using parport0 (interrupt-driven).
  ```

  If you have something like this, with only one address (0x378), then PDD will likely work. You can continue to the setup of parport symbolics before running PHANToMTest.

- ###### Case 2 :

  ```shell
  $ dmesg | grep parp
  [    6.643385] parport_pc 00:02: reported by Plug and Play ACPI
  [    6.643447] parport0: PC-style at 0x378 (0x778), irq 7, using FIFO [PCSPP,TRISTATE,COMPAT,EPP,ECP]
  [    6.748184] lp0: using parport0 (interrupt-driven).
  ```

  If you have something like this, with two addresses (0x378 and 0x778 in this case), and `using FIFO` , then PDD will likely not work. To resolve this, you have to tell Ubuntu to configure the parallel port with the same address for parallel port and ECR registers (the number in brackets).

  To do so, assuming you just work with one parallel port, the built-in one, just copy the file `parport.conf` from the git repository to`/etc/modprobe.d/` folder with root permission.

  You will have to edit the file if you want to change the default address, add another parallel port, or edit  the configuration for your PCI parallel port card if you have the same issue.

  You can find more info about this [here][PARP_CONF_1] and [here][PARP_CONF_2].

  Reboot the computer, and check the configuration with `dmesg | grep parp`, it should be good now.



**Back to the configuration** of the parallel port symbolic before running PHANToMTest.

PHANToM does not use parport`x` directly, instead it access the parport through a symbolic link named as `/dev/phnepp`.
So you have to create symbolic link named `/dev/phnepp` which points to `/dev/parportX`. In this tutorial, `/dev/phnepp` points to `/dev/parport0`.

If you have no idea about symbolic link, see '[what is link?][]'.

><u>NOTE</u> : The PHANTOM parallel port driver uses the /dev/phnepp file to
>access the hardware device. This is installed as a symbolic link to /dev/
>parport0. To control a PHANTOM attached to a different port, change
>this symbolic link to point to the appropriate device entry.
>Additionally, the “modes” file should be linked to /dev/phnepp_modes so
>that the selected port can be opened with the proper settings.
>--[PDD and OH Installation Guide For Linux][]--

**To create symbolic links**, run the following commands.

```shell
$ sudo chmod a+rw /dev/parport0
$ sudo ln -s /dev/parport0 /dev/phnepp
$ sudo ln -s /proc/sys/dev/parport/parport0/modes /dev/phnepp_modes
```

Finally, Phantom Premium 1.5 is ready to be run.
```shell
$ sudo /usr/sbin/PHANToMTest
```

![image_premium_1.5_test][]

![image_premium_1.5_box_test][]

Because our control board had been broken, I couldn't check button is working.

Excepting the button, everything(encoders, gimbal encoders, force feedback) was checked and working fine.

## Installing Open Haptics
Move to the OpenHaptics folder and install OpenHaptics.

>x86
```shell
$ cd [DOWNLOAD_DIR]/OpenHapticsAE_Linux_v3_0/OpenHapticsAE_Linux_v3_0/OpenHaptics-AE 3.0/32-bit
$ sudo dpkg -i openhaptics-ae_3.0-2_i386.deb
```
>x64
```shell
$ cd [DOWNLOAD_DIR]/OpenHapticsAE_Linux_v3_0/OpenHapticsAE_Linux_v3_0/OpenHaptics-AE 3.0/64-bit
$ sudo dpkg -i openhaptics-ae_3.0-2_amd64.deb
```

Then install `libncurse5-dev` library.
```shell
$ sudo apt-get install libncurses5-dev
```

Add 3DTOUCH path to `/etc/environment` file.
First open `environment` file with text editor.
```shell
$ sudo gedit /etc/environment
```
Then add the following line at the end of the file.
```
3DTOUCH_BASE=/usr/share/3DTouch
```
Then save and close.

## Test code
After installing OpenHaptics, you can find example codes at `/usr/share/3DTouch/examples`.
Copy examples source codes to you want or just use included in this repository.
Example codes in this repository are exactly same codes which are installed with OpenHaptics.

To create binary executable, move to a example you want.
```shell
$ cd [COPYED_DIR]/sensable_phantom_in_linux/examples/HD/graphics/ParticleWaltz
$ make
g++ -W -fexceptions -g -D_DEBUG -Dlinux -o ParticleWaltz ActorDynamics.cpp DynamicsSimulator.cpp ForceModel.cpp helper.cpp main.cpp -lHD -lHDU -lrt -lGL -lGLU -lglut -lncurses
```

Then you can execute binary.
```shell
$ ./ParticleWaltz
```

**HDAPI example(HD/ParticleWaltz)**

![image_example_hd_particle_waltz][]

**HLAPI example(HL/PointManipulation)**

![image_example_hl_point_manipulation][]


>If you come across with a similar error message as follows when you run example programs in x64 Ubuntu,
```shell
$ ./ParticleWaltz
./ParticleWaltz: error while loading shared libraries: libHD.so.3.0: cannot open shared object file: No such file or directory
```

>follow this commands.
```shell
$ sudo echo "/usr/lib64" | sudo tee /etc/ld.so.conf.d/open_haptics_libs.conf
$ sudo ldconfig
```

>close command window and reopen and try running the example again.

>You can find relevant articles [here][stkflw_shared_object_error], [here][stkflw_LD_LIBRARY_PATH], and [here][sudo_echo].

## Miscellaneous

* Phantom Device Driver library
    >The PHANTOM Device Driver library is built as a shared object and is installed in
 **/usr/lib/libPHANToMIO.so**

* Phantom Applications
  
>Administrative applications are installed in **/usr/sbin/**

* Phantom Configuration Files
    >The default directory for configuration files is
 **/etc/SensAble/PHANToMDeviceDrivers/**

* System Language

    >If your system language is not English, you have to prefix a flag `LANG=en_us`.
    >So when you run configuration program, you have to type `LANG=en_us /usr/sbin/PHANToMConfiguration`.
    >When running test program, you have to type `LANG=en_us /usr/sbin/PHANToMTest`.

## References

>coming soon...



## TODO

* Check if PDD is working with a [PCI parallel port card][ISSUE#3].
* Check is phantom programs work with the latest versions of Ubuntu.
* Investigate why [example codes cannot be compiled][ISSUE#1].
* Check if PDD can work with latest [OpenHaptics for Linux Developer Edition 3.4][OHLDE3.4].





[//]: # "======= LIST OF LINKS======="





[Sensable DSC]:http://dsc.sensable.com/3dtouch/openhaptics_academic_linux/index.asp
[PDD and OH Installation Guide For Linux]:http://lars.mec.ua.pt/public/LAR%20Projects/RobotNavigation/2015_MiguelRodrigues/workincopies/lar5/src/bases/phua_haptic/OpenHapticsAE_Linux_v3_0/HW_userguide_Linux.pdf
[what is link?]:http://www.computerhope.com/unix/uln.htm
[Phantom Omni Package Installation on ROS Fuerte & ROS Groovy]:http://robotica.unileon.es/mediawiki/index.php/Alvaro-RV-HAPTICROS01
[DSC_forum_JUJU_PDD]:http://developer.geomagic.com/viewtopic.php?f=2&t=1283
[DSC_forum_libGLw]:http://developer.geomagic.com/viewtopic.php?f=2&t=1241

[Sensable_Phantom_libs.tar.gz]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/Sensable_Phantom_libs.tar.gz

[stkflw_shared_object_error]:http://stackoverflow.com/questions/23991830/libthrift-0-9-1-so-cannot-open-shared-object-file-no-such-file-or-directory
[stkflw_LD_LIBRARY_PATH]:http://stackoverflow.com/questions/13428910/how-to-set-the-environmental-variable-ld-library-path-in-linux
[sudo_echo]:https://blogs.oracle.com/joshis/entry/sudo_echo_does_not_work
[zip_link]: http://lars.mec.ua.pt/public/LAR%20Projects/Humanoid/2012_PedroCruz/OPENHAPTICS%20v3/Driver%20e%20OpenHaptics%20LINUX/OpenHapticsAE_Linux_v3_0.zip	"Link to PDD and OpenHaptic 3.0 zip"
[ISSUE#1]: https://github.com/jaejunlee0538/sensable_phantom_in_linux/issues/1
[ISSUE#3]: https://github.com/jaejunlee0538/sensable_phantom_in_linux/issues/3
[OHLDE3.4]: https://3dssupport.microsoftcrmportals.com/knowledgebase/article/KA-03284/en-us

[LINK_GLX_ISSUE]: https://askubuntu.com/questions/745135/how-to-enable-indirect-glx-contexts-iglx-in-ubuntu-14-04-lts-with-nvidia-gfx

[PARP_CONF_1]: https://ubuntuforums.org/showthread.php?t=2258148
[PARP_CONF_2]: http://umax1220p.sourceforge.net/trouble.html



[//]: # "======= LIST OF IMAGES ======="





[image_bios_epp_setup]:https://i.ytimg.com/vi/YfirHQxmKcU/maxresdefault.jpg
[image_omni_configuration]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/phantom_omni_config.png
[image_omni_test]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/phantom_omni_test.png
[image_omni_box_test]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/phantom_omni_box_test.png
[image_premium_1.5_configuration]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/phantom_premium_config.png
[image_premium_1.5_test]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/phantom_premium_test.png
[image_premium_1.5_box_test]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/phantom_premium_box_test.png
[image_example_hd_particle_waltz]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/example_hd_particle_waltz.png
[image_example_hl_point_manipulation]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/example_hl_point_manipulation.png
[locate_xorg_conf]: https://raw.githubusercontent.com/Copper-Bot/sensable_phantom_in_linux/update-readme/resources/locate_xorg_conf.png

