# BBB_QT5_guide
This is a step by step guide for cross compiling Qt5.x on BeagleBone Black. The reason I made this guide is because the resources for cross compiling Qt5.x for BeagleBone Black platform are there but you've to dig deep into forums and do your research for a long time than actually compiling it. So, don't take this as *the only guide out there*, but something that can give you a headstart and a basic idea to start cross compilation.

### Prerequisites:
---------
- A BeagleBone Black (obviously.)
- Latest source code for Qt-everywhere from [official Qt archive](https://download.qt.io/archive/qt/) (For example: I'm going to use 5.12.1 throughout this guide which I got from [here](https://download.qt.io/archive/qt/5.12/5.12.1/single/))
- Latest Toolchain and Sysroot for gnu-armeabihf architecture (I got mine from [Linaro](https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/)) 
`WARNING: Make sure you use sysroot and toolchain from same source and they're of same version. If you want to genearate sysroot from your BBB board, the procedure is given in the official BBB beginner's guide (See sources at the end).`
- [Qt creator](https://www.qt.io/offline-installers) with same Qt version or below than Qt-everywhere source to installed. 

### Sources:
---------
- [BeagleBone Black beginner's guide to cross compiling Qt](https://wiki.qt.io/BeagleBone_Black_Beginners_Guide) - Quite old now but still only comprehensive guide mentioning all the steps in deep.
- [Yongli-aus' guide for Qt4.8.6](https://github.com/yongli-aus/qt-4.8.6-cross-compile-for-beaglebone-black) - The outline of steps is perfect even though Qt version update Qt4 -> Qt5 has changed a lot of commands and configuration.
- [Official Qt forum](https://forum.qt.io/topic/75565/i-need-a-qmake-for-the-beaglebone-black/13) - I can quote about 100 links from forums which have the required information scattered all over the place, but this specific thread has good discussion about the topic i.e. cross-compilation.
- [Stack overflow](https://stackoverflow.com/questions/44529558/qt-configure-issue-with-cross-compilation-device-and-xplatform) - Again, stack overflow has tons of threads but this specific one has *configure* command guidelines.

**In case if I've missed some link or might've shared your work without proper credits, then its by honest mistake. Please let know so I can fix errors.**