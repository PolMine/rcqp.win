---
title: "Crosscompilation for rcqp.win"
author: "Andreas Blätte (andreas.blaette@uni-due.de)"
date: "`r Sys.Date()`"
output: rmarkdown::html_vignette
vignette: >
  %\VignetteIndexEntry{Introduction to polmineR}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
---


# 32bit

The dll files in the inst/libs directory are copied from the rcwb project at
R-Forge: https://r-forge.r-project.org/scm/viewvc.php/libs/i386/?root=rcwb
They have been prepared by Bernard Desgraupes and Sylvain Loiseau.

# 64 bit

For a 64-bit R version, dll files needed to be obtained from other sources.

libiconv-2.dll and libintl-8.dll:
https://mlocati.github.io/articles/gettext-iconv-windows.html

libglib-2.0-0.dll:
http://www.dlldownloader.com/libglib2-00-dll/
  
pcre:
* using the file cross_compile in /Users/blaette/Lab/tmp/rcwb/Extras/Scripts/Shell
* commented out: cross_common.inc
* content of the file 'rcqpcrossrc':
RCQP_CROSS_CC=i386-mingw32-gcc
RCQP_CROSS_HOST=i386-mingw32
RCQP_CROSS_ARCH=i386
RCQP_CROSS_BASEDIR=~/Lab/tmp/CrossCompilation
RCQP_PCRE_URL=ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz
* call: ./cross_compile pcre

./configure --host=i386-mingw32-gcc --prefix=/Users/blaette/Lab/tmp/CrossCompilation/Output RCQP_PLATFORM=mingw RCQP_CROSS_CC=i386-mingw32-gcc RCQP_CROSS_HOST=i386-mingw32 RCQP_CROSS_ARCH=i386

A helpful explanation:
www.blogcompiler.com/2010/07/11/compile-for-windows-on-linux


### Cross-compiling pcre

```{sh}
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz
tar xzfv
cd pcre-DIR
```

To get the installation directory for cross-compilation:

```{r}
MINGW_OUTPUT_TMP=`x86_64-w64-mingw32-gcc --print-search-dirs | grep ^install`; echo ${MINGW_OUTPUT_TMP:9}
```


Make pcre on Dell:

```{r}
CC=x86_64-w64-mingw32-gcc CC_FOR_BUILD=gcc  ./configure             \
--host=x86_64-w64-mingw32                                  \
--enable-utf8 --enable-unicode-properties --enable-jit     \
--enable-newline-is-any --disable-cpp --enable-static      \
--prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/              \
--exec-prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/         \
--oldincludedir=/usr/lib/gcc/x86_64-w64-mingw32/4.8//include
```


libiconv
--------

```{r}
wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.15.tar.gz
tar xzfv libiconv-1.15.tar.gz
CC=x86_64-w64-mingw32-gcc CC_FOR_BUILD=gcc                            \
./configure                                                           \
--host=x86_64-w64-mingw32                                    \
--prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/                \
--exec-prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/           \
--oldincludedir=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include
make 
sudo make install
```

expat
-----

```{r}
wget http://downloads.sourceforge.net/expat/expat-2.1.0.tar.gz
tar xzfv expat-2.1.0.tar.gz
CC=x86_64-w64-mingw32-gcc CC_FOR_BUILD=gcc                            \
./configure                                                           \
--host=x86_64-w64-mingw32                                    \
--prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/                \
--exec-prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/           \
--oldincludedir=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include
make 
sudo make install
```

gettext
-------

```{r}
wget http://ftp.gnu.org/pub/gnu/gettext/gettext-latest.tar.gz
tar xzfv gettext-latest.tar.gz
cd gettext-0.19.8.1/
  CC=x86_64-w64-mingw32-gcc CC_FOR_BUILD=gcc                            \
./configure                                                           \
--host=x86_64-w64-mingw32                                    \
--prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/                \
--exec-prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/           \
--oldincludedir=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include
make
sudo make install
```

This failed often - at the end, it worked. Because it depends on glib ? no idea.. 



libffi
------

```{r}
wget ftp://sourceware.org/pub/libffi/libffi-3.2.tar.gz
tar xzfv libffi-3.2.tar.gz
cd libffi-3.2
LIBFFI_CFLAGS=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include             \
LIBFFI_LIBS=/usr/lib/gcc/x86_64-w64-mingw32/4.8/lib                   \
CC=x86_64-w64-mingw32-gcc CC_FOR_BUILD=gcc                            \
./configure                                                           \
--host=x86_64-w64-mingw32                                    \
--prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/                \
--exec-prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/           \
--oldincludedir=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include
make 
sudo make install
```

### Cross-compiling glib


HERE YOU CAN GET BINARIES! BEST CHOICE!!
http://ftp.gnome.org/pub/gnome/binaries/win64/glib/2.26/
  
AND HERE YOU GET libglib-2.0.0.dll !
wget http://ftp.gnome.org/pub/gnome/binaries/win64/glib/2.26/glib_2.26.0-1_win64.zip

```{sh}
wget http://ftp.gnome.org/pub/gnome/sources/glib/2.50/glib-2.50.0.tar.xz
tar xf glib-2.50.0.tar.xz
cd glib-2.50.0/
  PATH=$PATH:/usr/lib/gcc/x86_64-w64-mingw32/4.8/bin
export PATH
```

```{sh}
CFLAGS="-I/usr/lib/gcc/x86_64-w64-mingw32/4.8/include"                \
LIBFFI_CFLAGS=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include             \
LIBFFI_LIBS=/usr/lib/gcc/x86_64-w64-mingw32/4.8/lib                   \
CC=x86_64-w64-mingw32-gcc CC_FOR_BUILD=gcc  ./configure               \
--host=x86_64-w64-mingw32                                    \
--prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/                \
--exec-prefix=/usr/lib/gcc/x86_64-w64-mingw32/4.8/           \
--oldincludedir=/usr/lib/gcc/x86_64-w64-mingw32/4.8/include
```