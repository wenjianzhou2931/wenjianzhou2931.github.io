---
layout: article
title: Install Mitsuba on MacOS Big Sur
---

This is a document recording my experience when installing Mitsuba 0.6 on my Macbook. Hope it will help you!

Mitsuba 0.6 official website: http://www.mitsuba-renderer.org/index_old.html

Github page: https://github.com/mitsuba-renderer/mitsuba

## Environment

* Python2.7, you can use anaconda to create one, because Mitsuba is written in Python2:

```
$ conda create -n mitsuba_env python=2.7
$ conda activate mitsuba_env
```

* Scons (I use V3.1.1)

```
$ pip install SCons
```

or

```
$ conda install -c anaconda scons
```

* Xcode (V13.2.1) and Xcode Command Line Tools

To install command line tools, open Xcode -> Open Dev Tool -> More Dev Tools and install the correct version for your Xcode.

## Installation

1. Clone Mitsuba 0.6.0 repo:

```
$ git clone --recursive https://github.com/mitsuba-renderer/mitsuba.git
```

2. Go to your cloned directory (mitsuba), clone Mitsuba dependencies V0.6.0 for Mac:

```
$ git clone https://github.com/mitsuba-renderer/dependencies_macos.git
```

But don't use the dependencies link provided in official Mitsuba document because it's for Mitsuba V0.5.0

3. Change the name of dependency folder:

```
$ mv dependencies_macos dependencies
```

4. Copy /mitsuba/build/config-macos10.12-clang-x86_64.py configuration file into the root /mitsuba directory:

```
$ cp ./build/config-macos10.12-clang-x86_64.py config.py
```

5. Open config.py and change from:

```
PYTHON27INCLUDE= ['/System/Library/Frameworks/Python.framework/Versions/2.7/Headers']
```

to

```
PYTHON27INCLUDE= ['/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7/']
```

6. Open config.py and change the MacOS SDK path from:

```
'/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk'
```

to

```
'/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk'
```

Remember that you should make the change in both CCFLAGS and LINKFLAGS.

7. Go to /mitsuba/dependencies folder, open the file named 'version', delete everything in it (which is a string '0.6.0').

8. Create a file 'version.txt' and input '0.6.0'(also in the dependencies folder).

9. Go to /mitsuba/build folder, open SConscript.configure, in line 283, change

```
versionFilename = GetBuildPath('#dependencies/version')
```

to

```
versionFilename = GetBuildPath('#dependencies/version.txt')
```

10. Head back to /mitsuba folder, run Scons to compile Mitsuba

```
$ scons
```

It sould take several minutes to finish.

11. Set path for mitsuba

```
$ source setpath.sh
```

Now you can start rendering with Mitsuba!

If you have any question, please email me:

```
wjchou2931@gmail.com
```

Wenjian Zhou

## Reference

* https://github.com/ShilinC/Mitsuba_Install_MacOS

