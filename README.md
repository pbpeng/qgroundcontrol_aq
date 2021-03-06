# QGroundControl for AutoQuad
### Open Source Micro Air Vehicle Ground Control Station
#### Modified for the AutoQuad flight controller and ESC32 motor controller.

* [AutoQuad Project](http://autoquad.org/)

* [AutoQuad Forums](http://forum.autoquad.org)

* [QGC AQ Release Notes](http://autoquad.org/wiki/wiki/whats-new/autoquad-software-changes/)

* AutoQuad Compiled Software and Firmware Downloads:
  * <http://autoquad.org/software-downloads/>
  * <ftp://ftp.autoquad.org/3/334693_22529/>

* [Original QGroundControl Project](http://qgroundcontrol.org) and [Credits](http://qgroundcontrol.org/credits)


## Obtaining source code
There are three ways to obtain the QGroundControl source code from our GitHub repository. You can either download a snapshot of the code in a ZIP file, clone the repository, or fork the repository if you plan to contribute to development. If you prefer one of the last two options you will need Git installed on your system; goto GitHub Help and see Set Up Git.

**If you use Git**, make sure to **update submodules** after a clone or updates pull. This will pull in the latest MAVLink headers from another repo.

**If you download an archive**, you will also need to download the latest version of https://github.com/AutoQuad/mavlink_headers and place the content in the `libs/mavlink` folder of QGC source code.

### Clone the Repository
This option assumes that you have Git already. To clone (checkout) the QGC repository, run the following command in the directory that you want the qgroundcontrol_aq folder to be created:

```
git clone https://github.com/AutoQuad/qgroundcontrol_aq.git
cd qgroundcontrol_aq
git submodule update --init
```

## Building QGroundControl
QGroundControl builds are supported for OSX, Linux, and Windows. Qt versions 4.8.6 and 5.x are supported (Qt5 recommended, tested up to 5.5.1 at time of writing). See the individual sections below for specific requirements for each OS.

In general we recommend installing the latest Qt libraries and development environment (QtCreator), as this will provide the simplest and most tested build setup.

**If you get errors about missing MAVLink variables/functions** then you need to read the part above about git *submodule update* or *downloading the mavlink headers* manually.

**To build without Text-To-Speech** you need to add "NO_TEXT_TO_SPEECH" to your
Qt DEFINES variable.  You can do this by un-commenting "#DEFINES += NO_TEXT_TO_SPEECH" in qgroundcontrol_aq.pro or adding the DEFINE line to your build configuration in QtCreator.  On Linux, speech support will be automatcially disabled if the required development files are not found.

### Common Instructions

#### Install QT
- - -
1. Download and install Qt 5 (including QtCreator) for your OS from: <http://www.qt.io/download-open-source/> (if the site doesn't detect your Linux operating system correctly, find the "manual download" option and choose the right one yourself)
2. Choose the Qt flavor which matches your development environment, see OS-specific notes below.
3. Install Qt as per their instructions.

#### Open and configure the project
1. Open the qgroundcontrol_aq.pro file (at the root of this project) in QtCreator.
2. This step will ensure all necessary supporting files are copied to the right folder after a successful build: In the project Build Settings, expand the "Make" step and in the "Make arguments" field enter: `release install` (or `debug install` for debug type build).  You may need to add a "Make" step if there isn't one listed already under the "qmake" step.
3. Assuming all the requirements below are met, you should be able to initiate a build (big green button on lower left of QtCreator).  Make sure you first select the proper build type (Release or Debug) using the menu just above the green arrow button.

### Build on Linux
Supported builds for Linux are 32 or 64-bit, built using gcc.

#### Install gcc, SDL1.2, udev.
* Ubuntu/Debian: `sudo apt-get install build-essential libsdl1.2-dev libudev-dev`
* For Fedora: `sudo yum install systemd-devel SDL-devel SDL-static`

#### Install Qt5 (Qt can also be installed via download as described above, **recommended**).
* For Ubuntu (requires 14.10 for Qt5.3): `sudo apt-get install qtcreator qttools5-dev qtbase5-dev qt5-default qtdeclarative5-dev libqt5svg5-dev libqt5webkit5-dev qtmultimedia5-dev`
* For Fedora: `sudo yum install qt-creator qt5-qtbase-devel qt5-qtdeclarative-devel qt5-qtsvg-devel qt5-qtwebkit-devel qt5-multimedia-devel`

#### [Optional] Install additional libraries for text-to-speech support
* For text-to-speech (festival)
	* For Ubuntu: `sudo apt-get install festival-dev festival festvox-en1`
	* For Fedora: `sudo yum install festival-dev festival festvox-en1`

#### Build QGroundControl
1. Build using QtCreator as described above.  Alternately, build from the command line:
 1. Change directory to you `qgroundcontrol_aq` source directory.
 2. Run `qmake`
 3. Run `make`

**See README-Linux.txt to check instructions for running QGC on your system.**

### Build on Mac OSX

By default requires clang 64-bit compiler typical on modern OS X.  Compilation has been tested up to OS X Mavericks.  You shouldn't need anything else.  Just open the project file in QtCreator and build, as described above.

Note that the Google Earth plugin does not work with 64-bit builds. To buld a version with a working Google Earth plugin, one must use 32 bit Qt libraries. At the time of this writing, this means building the libraries from source, using the ./configure -arch x386 option.

### Build on Windows
Supported builds for Windows are 32bit (64b might work, not tested), using MSVC 2010 or higher.  MinGW/GCC might work, but not tested.

_Currently to build with Speech support (QtSpeech) requires MSVC 2010 (VC10) compiler (either Visual Studio 2010 or Windows SDK 7.1)._

_Apparent bug with Qt 5.4 and MSVC 2013: You must revert this change: https://qt.gitorious.org/qt/qtbase/commit/9f0e5d00ab51cc7c0dc87c8d72f48c4e6eda   This file can be found in your Qt install folder, eg: Qt/5.4/msvc_2013_opengl/mkspecs/win32-msvc2013/qmake.conf.  Remove the "-Zc:strictStrings" part from both lines (28 and 29)._

#### Install Visual Studio Express (2010 or newer) or Windows SDK (7.1 or 8.x)
Obtain an MSVC development environment (compiler/linker/etc) if you don't have one yet.  The simplest way it probably to install a Visual Studio package, although the SDKs provide similar functionality but w/out the massive overhead of the IDE (editor) which you don't really need.  If using VS, make sure you install the Windows Desktop version.

#### Qt Variant
When installing Qt, you need to choose which compiler you're using, and how many bits.  Choose the variant that matches your MSVC version, and pick the 32-bit variant with OpenGL support.

#### Build QGroundControl

Optional for Text To Speech: 
 * First follow the instructions in libs/QtSpeech/INSTALL.txt (in the qgroundcontrol_aq root folder).
 * If you do not have Visual Studio PRO version installed, you also need the ATL MFC developer package.  The simplest way to get this is to download the Windows Driver Kit Version 7.1.0 (http://www.microsoft.com/en-us/download/details.aspx?id=11800).  Mount the ISO and run the installer, then select the "Build Environments" feature (also keep Debuggin Tools checked if it is already).  Install it to the folder of your choice (you can remove it later).  Once installed, find the inc/atl71 folder and copy the contents to C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\atlmfc.  Then find the lib/ATL/i386 folder and copy the contents to C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\lib.  (For 32-bit systems, adjust the paths accordingly and then edit the path definitions in the /libs/QtSpeech/qtspeech.pri file.)

##### Option A: Qt Creator
Simply open the qgroundcontrol_aq.pro file in QtCreator, adjust the build directories on the opening screen if needed.  You should be able to simply build the project now using the Build button or CTRL-B.

##### Option B: Visual Studio
1. Open the Qt Command Prompt program from the Start Menu
2. Change directory to your 'qgroundcontrol_aq' source folder.
3. Run `qmake -tp vc qgroundcontrol.pro`.  This will create a 'qgroundcontrol.vcxproj' project file which is capable of building both debug and release configurations.
4. Now open the generated 'qgroundcontrol.vcxproj' file in Visual Studio.
5. Compile and edit in Visual Studio. If you need to add new files, add them to src/src.pri and re-run qmake from step 3.


## Credits

QGroundControl Creator:
Lorenz Meier <lm@inf.ethz.ch>
Copyright 2009-2011 QGroundControl Developers

AutoQuad Maintainer:
Maxim Paperno <MPaperno@WorldDesign.com>
Copyright 2013-2016 Maxim Paperno

Original Conversion for AutoQuad
Copyright 2012-2013 by Peter Hafner

## License

This file is part of the open source AutoQuad project.
QGroundControl AQ is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.
QGroundControl AQ is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.
You should have received a copy of the GNU General Public License
along with QGroundControl AQ. If not, see <http://www.gnu.org/licenses/>.
