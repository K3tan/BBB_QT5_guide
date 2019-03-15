# BBB_QT5_guide
This is a step by step guide for cross compiling Qt5.x on BeagleBone Black. The reason I made this guide is because the resources for cross compiling Qt5.x for BeagleBone Black platform are there but you've to dig deep into forums and do your research for a long time than actually compiling it. So, don't take this as *the only guide out there*, but something that can give you a headstart and a basic idea to start cross compilation.

### Prerequisites:
- A BeagleBone Black (obviously.)
- Latest source code for Qt-everywhere from [official Qt archive](https://download.qt.io/archive/qt/) (For example: I'm going to use 5.12.1 throughout this guide which I got from [here](https://download.qt.io/archive/qt/5.12/5.12.1/single/))
- Latest Toolchain and Sysroot for gnu-armeabihf architecture (I got mine from [Linaro](https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/)) 
`WARNING: Make sure you use sysroot and toolchain from same source and they're of same version. If you want to genearate sysroot from your BBB board, the procedure is given in the official BBB beginner's guide (See sources at the end).`
- [Qt creator](https://www.qt.io/offline-installers) with same Qt version or below than Qt-everywhere source to installed. 

### Procedure:
##### 1. Extract the downloaded Qt source file in your desired directory
```sh
unxz qt-everywhere-src-5.12.1.tar.xz
tar -xvf qt-everywhere-src-5.12.1.tar
```

##### 2. Extract and copy toolchain alongwith sysroot to desired directory
```sh
unxz gcc-linaro-x.x.x-yyyy.mm-x86_64_arm-linux-gnueabihf.tar.xz
tar -xvf gcc-linaro-x.x.x-yyyy.mm-x86_64_arm-linux-gnueabihf.tar

unxz sysroot-glibc-linaro-x.xx-yyyy.mm-arm-linux-gnueabihf.tar.xz
tar -xvf sysroot-glibc-linaro-x.xx-yyyy.mm-arm-linux-gnueabihf.tar
```

##### 3. Move toolchain and sysroot to its desired path /usr/local/linaro
```sh
sudo cp gcc-linaro-x.x.x-yyyy.mm-x86_64_arm-linux-gnueabihf /usr/local/linaro/
sudo cp sysroot-glibc-linaro-x.xx-yyyy.mm-arm-linux-gnueabihf /usr/local/linaro/
```

In the previous versions of Qt i.e. 4.x, we had to create a new mkspec to generate qmake for arm-gnueabihf platform, but in 5.x versions, Qt itself contains support for both arm-gnueabihf platform and Beaglebone device, so that step is obsolete now and our work is easier.

##### 4. Configure Qt for BBB
Go to qt-everywhere extracted directory and run ./configure. It should look something like the one below but you can always customize the parameters anytime as per official [configure options](https://doc.qt.io/qt-5/configure-options.html).

```sh
./configure -platform linux-g++ -release -device linux-beagleboard-g++ \ 
-sysroot /usr/local/linaro/sysroot-glibc-linaro-x.xx-yyyy.mm-arm-linux-gnueabihf \
-prefix ~/Qt5ForBBB -hostprefix ~/Qt5forBBB \
-device-option CROSS_COMPILE=/usr/local/linaro/gcc-linaro-x.x.x-yyyy.mm-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf- \
-nomake tests -nomake examples -no-opengl -opensource -confirm-license -reduce-exports -make libs 

```
Parameters:
```tex
-platform:       Specifies the workstation platform,
-device:         Target device (list of devices available in qt-everywhere/qtbase/mkspecs/devices),
-sysroot:        Specifies the directory in which sysroot is available,
-prefix:         Specifies the directory on workstation where compiled Qt and libs will be stored,
-hostprefix:     Specifies the directory on BBB where target executables will be exported,
-device-option:  Gives the location of toolchain needed for CROSS_COMPILE,
```
After successfully executing the configure command, you'll have Makefile in *qt-everywhere* directory.

##### 5. make and make install
First run `make` or `make -j4` in case you need multi threading for faster compilation. You can ignore the warnings as they won't affect your actual installation or execution of Qt on BBB. After successful completion of `makefile`,  you need to run the command `sudo make install`. Be aware of *sudo* as it is necessary to make the directory specified in *-hostprefix* parameter in ./configure.   

##### 6. Configure Qt Creator
- To select device as BeagleBone:
  Go to `Tools` > `Options` > `Devices` and edit as follows:
  
  ![](https://imgur.com/4qWRRb3l.png)

- Then go to `Tools` > `Options` > `Kits` > `Compilers` and add both C and C++ compilers as gcc-arm-gnuabihf-directory/bin/arm-gnuabihf-g++.
  
  ![](https://imgur.com/wONE05zl.png)

- Set Qt version in Qt creator:
  Go to `Tools` > `Options` > `Kits` > `Qt versions` > `Add` and browse the qmake /bin/ folder in Qt installation directory specified with `-prefix` parameter above in configuration step. (For example: in my case it was in *~/Qt5forBBB/bin/qmake*)

- Final step - add BeagleBone as Kit in Qt Creator:
  To combine all the settings above, go to `Tools` > `Options` > `Kits` and then click `Add`. Configure as follows:
  ```sh
  Name:                 BeagleBone (or as you wish.)
  Device type:          Generic Linux Device
  Device:               BBB (As configured above in step 6.1)
  Compiler C & C++:     As configured in step 6.2
  Debugger:             Use gdb-multiarch
  Qt version:           Same one from step 6.3
  ``` 
  Then click `Apply` and `Ok`.

##### 7. Project creation
When you create a new project, select your BeagleBone Kit as target, so that Qt Creator can remotely deploy and debug the running code from your workstation.

--------

I hope that you've got a much better idea at cross compiling Qt5 for BeagleBone Black. The only purpose of this writeup is to help absolute beginners to Qt like myself to get a headstart rather than wandering and searching for hours on internet before finding some solutions. Feel free to suggest changes in this article if I've made some mistake or I've given some wrong instruction. 

--------
### Sources:
- [BeagleBone Black beginner's guide to cross compiling Qt](https://wiki.qt.io/BeagleBone_Black_Beginners_Guide) - Quite old now but still only comprehensive guide mentioning all the steps in deep.
- [Yongli-aus' guide for Qt4.8.6](https://github.com/yongli-aus/qt-4.8.6-cross-compile-for-beaglebone-black) - The outline of steps is perfect even though Qt version update Qt4 -> Qt5 has changed a lot of commands and configuration.
- [Official Qt forum](https://forum.qt.io/topic/75565/i-need-a-qmake-for-the-beaglebone-black/13) - I can quote about 100 links from forums which have the required information scattered all over the place, but this specific thread has good discussion about the topic i.e. cross-compilation.
- [Stack overflow](https://stackoverflow.com/questions/44529558/qt-configure-issue-with-cross-compilation-device-and-xplatform) - Again, stack overflow has tons of threads but this specific one has *configure* command guidelines.

**In case if I've missed some link or might've shared your work without proper credits, then its by honest mistake. Please let me know so I can fix errors.**