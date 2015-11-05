
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


