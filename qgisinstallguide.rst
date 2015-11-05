
Installing and Compiling QGIS
=============================

.. contents::
   :local:


Introduction
------------


This document is the original installation guide of the described software 
QGIS. The software and hardware descriptions named in this 
document are in most cases registered trademarks and are therefore subject 
to the legal requirements. QGIS is subject to the GNU General Public 
License. Find more information on the QGIS Homepage:
http://qgis.org

The details, that are given in this document have been written and verified 
to the best of knowledge and responsibility of the editors. Nevertheless, 
mistakes concerning the content are possible. Therefore, all data are not 
liable to any duties or guarantees. The editors and publishers do not take 
any responsibility or liability for failures and their consequences. You are 
always welcome for indicating possible mistakes.

You can download this document as part of the QGIS 'User and 
Installation Guide' in HTML and PDF format via http://qgis.org. A current 
version is also available at:
http://htmlpreview.github.io/?https://raw.github.com/qgis/QGIS/master/doc/INSTALL.html

Translations of this document can also be downloaded at the documentation area 
of the QGIS project at http://qgis.org. More information is 
available via http://qgis.org/en/site/getinvolved/governance/organisation/governance.html#community-resources. 

Please visit http://qgis.org for information on joining our mailing lists 
and getting involved in the project further.

/!\ Note to document writers: Please use this document as the central
place for describing build procedures. Please do not remove this notice. 

/!\ Note to document writers: This documented is generated from 
doc/INSTALL.t2t - if you need to edit this document, be sure to edit that 
file rather than the generated INSTALL document found in the root of the 
source directory.

Overview
--------


QGIS, like a number of major projects (eg. KDE 4.0), uses CMake
(http://www.cmake.org) for building from source.

Following a summary of the required dependencies for building:

Required build tools:

- CMake >= 2.8.6
- Flex >= 2.5.6
- Bison >= 2.4

Required build dependencies:

- Qt >= 4.8.0
- Proj >= 4.4.x
- GEOS >= 3.4
- Sqlite3 >= 3.0.0
- GDAL/OGR >= 1.4.x
- Qwt >= 5.0 & (< 6.1 with internal QwtPolar)
- expat >= 1.95
- QScintilla2

Optional dependencies:

- for GRASS providers and plugin - GRASS >= 6.0.0. QGIS may be compiled with GRASS 6 or GRASS 7.
  It can also be compiled with both GRASS versions in a single build but only if QGIS
  is not installed with rpath. The desired GRASS version is chosen on runtime by setting
  LD_LIBRARY_PATH or PATH.
- for georeferencer - GSL >= 1.8
- for postgis support and SPIT plugin - PostgreSQL >= 8.0.x
- for gps plugin - gpsbabel
- for mapserver export and PyQGIS - Python >= 2.3 (2.5+ preferred)
- for python support - SIP >= 4.12, PyQt >= 4.8.3 must match Qt version, Qscintilla2
- for qgis mapserver - FastCGI
- for oracle provider - Oracle OCI library

Indirect dependencies:

Some proprietary formats (eg. ECW and MrSid) supported by GDAL require
proprietary third party libraries.  QGIS doesn't need any of those itself to
build, but will only support those formats if GDAL is built accordingly.  Refer
to http://gdal.org/formats_list.html ff. for instructions how to include
those formats in GDAL.


Building on GNU/Linux
---------------------


Building QGIS with Qt 4.x
.........................


Requires: Ubuntu / Debian derived distro

/!\ Note: Refer to the section Building Debian packages for building
debian packages.  Unless you plan to develop on QGIS, that is probably the
easiest option to compile and install QGIS.

These notes are for Ubuntu - other versions and Debian derived distros may
require slight variations in package names.

These notes are for if you want to build QGIS from source. One of the major
aims here is to show how this can be done using binary packages for *all*
dependencies - building only the core QGIS stuff from source. I prefer this
approach because it means we can leave the business of managing system packages
to apt and only concern ourselves with coding QGIS!

This document assumes you have made a fresh install and have a 'clean' system.
These instructions should work fine if this is a system that has already been
in use for a while, you may need to just skip those steps which are irrelevant
to you.


Prepare apt
...........

The packages QGIS depends on to build are available in the "universe" component
of Ubuntu. This is not activated by default, so you need to activate it:

1. Edit your /etc/apt/sources.list file.
2. Uncomment all the lines starting with "deb"

Also you will need to be running Ubuntu 'precise' or higher in order for
all dependencies to be met.

Now update your local sources database::

  sudo apt-get update


Install build dependencies
..........................

Distribution: install command for packages

wheezy ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal1-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools python python-dev python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

jessie ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools pyqt4.qsci-dev python-all python-all-dev python-pyspatialite python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``
  
stretch ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools pyqt4.qsci-dev python-all python-all-dev python-pyspatialite python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

precise ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt5-qt4-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools python python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

trusty ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt5-qt4-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools python-all python-all-dev python-pyspatialite python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

utopic ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt5-qt4-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools python-all python-all-dev python-pyspatialite python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

vivid ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt5-qt4-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools python-all python-all-dev python-pyspatialite python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

sid ``apt-get install bison cmake doxygen flex git graphviz grass-dev libexpat1-dev libfcgi-dev libgdal-dev libgeos-dev libgsl0-dev libopenscenegraph-dev libosgearth-dev libpq-dev libproj-dev libqscintilla2-dev libqt4-dev libqt4-opengl-dev libqt4-sql-sqlite libqtwebkit-dev libqwt-dev libspatialindex-dev libspatialite-dev libsqlite3-dev lighttpd locales pkg-config poppler-utils pyqt4-dev-tools pyqt4.qsci-dev python-all python-all-dev python-pyspatialite python-qscintilla2 python-qt4 python-qt4-dev python-sip python-sip-dev qt4-doc-html spawn-fcgi txt2tags xauth xfonts-100dpi xfonts-75dpi xfonts-base xfonts-scalable xvfb cmake-curses-gui``

(extracted from the control.in file in debian/)


Setup ccache (Optional)
.......................

You should also setup ccache to speed up compile times::

  cd /usr/local/bin
  sudo ln -s /usr/bin/ccache gcc
  sudo ln -s /usr/bin/ccache g++


Prepare your development environment
....................................

As a convention I do all my development work in $HOME/dev/<language>, so in
this case we will create a work environment for C++ development work like
this::

  mkdir -p ${HOME}/dev/cpp
  cd ${HOME}/dev/cpp

This directory path will be assumed for all instructions that follow.


Check out the QGIS Source Code
..............................

There are two ways the source can be checked out. Use the anonymous method
if you do not have edit privileges for the QGIS source repository, or use
the developer checkout if you have permissions to commit source code changes.

1. Anonymous Checkout::

  cd ${HOME}/dev/cpp
  git clone git://github.com/qgis/QGIS.git

2. Developer Checkout::

  cd ${HOME}/dev/cpp
  git clone git@github.com:qgis/QGIS.git


Starting the compile
....................

I compile my development version of QGIS into my ~/apps directory to avoid
conflicts with Ubuntu packages that may be under /usr. This way for example
you can use the binary packages of QGIS on your system along side with your
development version. I suggest you do something similar::

  mkdir -p ${HOME}/apps

Now we create a build directory and run ccmake::

  cd QGIS
  mkdir build-master
  cd build-master
  ccmake ..

When you run ccmake (note the .. is required!), a menu will appear where
you can configure various aspects of the build. If you want QGIS to have
debugging capabilities then set CMAKE_BUILD_TYPE to Debug. If you do not have
root access or do not want to overwrite existing QGIS installs (by your
packagemanager for example), set the CMAKE_INSTALL_PREFIX to somewhere you
have write access to (I usually use ${HOME}/apps). Now press
'c' to configure, 'e' to dismiss any error messages that may appear.
and 'g' to generate the make files. Note that sometimes 'c' needs to
be pressed several times before the 'g' option becomes available.
After the 'g' generation is complete, press 'q' to exit the ccmake
interactive dialog.

Now on with the build::

  make
  make install

It may take a little while to build depending on your platform.

After that you can try to run QGIS::

  $HOME/apps/bin/qgis

If all has worked properly the QGIS application should start up and appear
on your screen.  If you get the error message "error while loading shared libraries",
execute this command in your shell::

  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:${HOME}/apps/lib/


Building Debian packages
........................

Instead of creating a personal installation as in the previous step you can
also create debian package.  This is done from the QGIS root directory, where
you'll find a debian directory.

First you need to install the debian packaging tools once:

  apt-get install build-essential

First you need to create an changelog entry for your distribution. For example for Ubuntu Lucid:

  dch -l ~precise --force-distribution --distribution precise "precise build"

The QGIS packages will be created with:

  dpkg-buildpackage -us -uc -b

/!\ Note: Install devscripts to get dch.

/!\ Note: If dpkg-buildpackage complains about unmet build dependencies
you can install them using apt-get and re-run the command.

/!\ Note: If you have libqgis1-dev installed, you need to remove it first
using dpkg -r libqgis1-dev.  Otherwise dpkg-buildpackage will complain about a
build conflict.

/!\ Note: By default tests are run in the process of building and their
results are uploaded to http://dash.orfeo-toolbox.org/index.php?project=QGIS.
You can turn the tests off using DEB_BUILD_OPTIONS=nocheck in front of the
build command. The upload of results can be avoided with DEB_TEST_TARGET=test.

The packages are created in the parent directory (ie. one level up).
Install them using dpkg.  E.g.::

  sudo debi


Building on Windows
-------------------


Building with Microsoft Visual Studio
.....................................

This section describes how to build QGIS using Visual Studio on Windows.  This
is currently also how the binary QGIS packages are made (earlier versions used
MinGW).

This section describes the setup required to allow Visual Studio to be used to
build QGIS. 


Visual C++ Express Edition
~~~~~~~~~~~~~~~~~~~~~~~~~~

The free (as in free beer) Express Edition installer is available under:

  http://download.microsoft.com/download/c/d/7/cd7d4dfb-5290-4cc7-9f85-ab9e3c9af796/vc_web.exe

You also need the Windows SDK for Windows 7 and .NET Framework 4:

  http://download.microsoft.com/download/A/6/A/A6AC035D-DA3F-4F0C-ADA4-37C8E5D34E3D/winsdk_web.exe


Other tools and dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download and install following packages:

Tool: Website

Make: http://www.cmake.org/files/v3.0/cmake-3.0.2-win32-x86.exe

GNU flex, GNU bison and GIT: http://cygwin.com/setup-x86.exe (32bit) or http://cygwin.com/setup-x86_64.exe (64bit)

OSGeo4W: http://download.osgeo.org/osgeo4w/osgeo4w-setup-x86.exe (32bit) or http://download.osgeo.org/osgeo4w/osgeo4w-setup-x86_64.exe (64bit)

OSGeo4W does not only provide ready packages for the current QGIS release and
nightly builds of master, but also offers most of the dependencies needs to
build it.

For the QGIS build you need to install following packages from cygwin:

- bison
- flex
- git

and from OSGeo4W (select Advanced Installation):

- expat
- fcgi
- gdal
- grass
- gsl-devel
- iconv
- pyqt4
- qt4-devel
- qwt5-devel-qt4
- sip
- spatialite
- libspatialindex-devel
- python-qscintilla

This will also select packages the above packages depend on.

Earlier versions of this document also covered how to build all above
dependencies.  If you're interested in that, check the history of this page in the Wiki
or the SVN repository.


Setting up the Visual Studio project with CMake
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

/!\ Consider this section as example.  It tends to outdate, when OSGeo4W and
SDKs move on.  ms-windows/osgeo4w/package-nightly.cmd is used for the
nightly builds and constantly updated and hence might contain necessary
updates that are not yet reflected here.

To start a command prompt with an environment that both has the VC++ and the OSGeo4W
variables create the following batch file (assuming the above packages were
installed in the default locations)::

  @echo off
  set VS90COMNTOOLS=%PROGRAMFILES%\Microsoft Visual Studio 9.0\Common7\Tools\
  call "%PROGRAMFILES%\Microsoft Visual Studio 9.0\VC\vcvarsall.bat" x86
  
  set INCLUDE=%INCLUDE%;%PROGRAMFILES%\Microsoft SDKs\Windows\v7.1\include
  set LIB=%LIB%;%PROGRAMFILES%\Microsoft SDKs\Windows\v7.1\lib
  
  set OSGEO4W_ROOT=C:\OSGeo4W
  call "%OSGEO4W_ROOT%\bin\o4w_env.bat"
  path %PATH%;%PROGRAMFILES%\CMake\bin;c:\cygwin\bin
  
  @set GRASS_PREFIX=c:/OSGeo4W/apps/grass/grass-6.4.4
  @set INCLUDE=%INCLUDE%;%OSGEO4W_ROOT%\include
  @set LIB=%LIB%;%OSGEO4W_ROOT%\lib;%OSGEO4W_ROOT%\lib
  
  @cmd

Start the batch file and on the command prompt checkout the QGIS source from
git to the source directory QGIS::

  git clone git://github.com/qgis/QGIS.git

Create a 'build' directory somewhere. This will be where all the build output
will be generated.

Now run cmake-gui (still from cmd) and in the Where is the source code:
box, browse to the top level QGIS directory.

In the Where to build the binaries: box, browse to the 'build' directory you
created.

If the path to bison and flex contains blanks, you need to use the short name
for the directory (i.e. C:\Program Files should be rewritten to
C:\Progra~n, where n is the number as shown in `dir /x C:\``).

Verify that the 'BINDINGS_GLOBAL_INSTALL' option is not checked, so that python
bindings are placed into the output directory when you run the INSTALL target.

Hit Configure to start the configuration and select Visual Studio 9 2008
and keep native compilers and click Finish.

The configuration should complete without any further questions and allow you to
click Generate.

Now close cmake-gui and continue on the command prompt by starting
vcexpress.  Use File / Open / Project/Solutions and open the
qgis-x.y.z.sln File in your project directory.

Change Solution Configuration from Debug to RelWithDebInfo (Release
with Debug Info)  or Release before you build QGIS using the ALL_BUILD
target (otherwise you need debug libraries that are not included).

After the build completed you should install QGIS using the INSTALL target.

Install QGIS by building the INSTALL project. By default this will install to
c:\Program Files\qgis<version> (this can be changed by changing the
CMAKE_INSTALL_PREFIX variable in cmake-gui). 

You will also either need to add all the dependency DLLs to the QGIS install
directory or add their respective directories to your PATH.


Packaging
~~~~~~~~~

To create a standalone installer there is a perl script named 'creatensis.pl'
in 'qgis/ms-windows/osgeo4w'.  It downloads all required packages from OSGeo4W
and repackages them into an installer using NSIS.

The script can be run on both Windows and Linux.

On Debian/Ubuntu you can just install the 'nsis' package.

NSIS for Windows can be downloaded at:

  http://nsis.sourceforge.net

And Perl for Windows (including other requirements like 'wget', 'unzip', 'tar'
and 'bzip2') is available at:

  http://cygwin.com


Packaging your own build of QGIS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Assuming you have completed the above packaging step, if you want to include
your own hand built QGIS executables, you need to copy them in from your
windows installation into the ms-windows file tree created by the creatensis
script.::

  cd ms-windows/
  rm -rf osgeo4w/unpacked/apps/qgis/*
  cp -r /tmp/qgis1.7.0/* osgeo4w/unpacked/apps/qgis/

Now create a package.::

  ./quickpackage.sh

After this you should now have a nsis installer containing your own build 
of QGIS and all dependencies needed to run it on a windows machine.


Osgeo4w packaging
~~~~~~~~~~~~~~~~~

The actual packaging process is currently not documented, for now please take a
look at:

ms-windows/osgeo4w/package.cmd


