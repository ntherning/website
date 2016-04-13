---
title: Compiling Mono on Windows
redirect_from:
  - /Compiling_Mono_on_Windows/
---

There are two ways of building Mono on Windows, both do require a Cygwin setup which provides some tools required by Mono at build time. [Chocolatey](https://chocolatey.org) makes it very easy to install Cygwin and the required packages. With Chocolatey installed run the following in cmd.exe (started as Administrator):

``` bash
choco install cygwin --version 2.4.1
choco install cyg-get
cyg-get.bat autoconf automake bison gcc-core gcc-g++ mingw64-i686-runtime mingw64-i686-binutils mingw64-i686-gcc-core mingw64-i686-gcc-g++ mingw64-i686-pthreads mingw64-i686-w32api mingw64-x86_64-runtime mingw64-x86_64-binutils mingw64-x86_64-gcc-core mingw64-x86_64-gcc-g++ mingw64-x86_64-pthreads mingw64-x86_64-w32api libtool make python gettext-devel gettext intltool libiconv pkg-config git curl libxslt
```

At this point, you can either do a [Visual Studio build](/docs/compiling-mono/windows/compiling-with-visualstudio/) or you can continue reading the instructions for a Cygwin build.

The following steps assume you're running in an instance of the Cygwin terminal. Pick an installation directory where you want your new copy of Mono to be installed. Lets call that location PREFIX. Your Mono installation and its dependencies will be installed there.

Building Mono 32-bit from a Release Package
-------------------------------------------

Mono releases are distributed as .tar.bz2 packages from the Mono web site. Once you have your dependencies installed all you need to do is run the following command where VERSION is the package version number and PREFIX is your installation prefix:

``` bash
PREFIX=/usr/local
VERSION=4.2.3
tar xvf mono-$VERSION.tar.bz2
cd mono-$VERSION
./configure --prefix=$PREFIX --host=i686-w64-mingw32
make
make install
```

By the end of this process, you will have Mono installed in your PREFIX directory.

Building Mono 32/64-bit From a Git Source Code Checkout
-------------------------------------------------------

To build Mono from a Git Source Code checkout, you will want to have the official Mono installed on the system and in your PATH, as the build requires a working C# compiler to run. Mono can be installed using Chocolatey (again in cmd.exe started as Administrator):

``` bash
choco install mono
```

On Windows, you should set the following Git setting to avoid issues with line endings: `git config --global core.autocrlf input`

Once you do this, Mono can be built either in 32-bit or 64-bit mode. When running the following commands, remember to replace PREFIX with your installation prefix that you selected.

Mono 32-bit build:
``` bash
PATH=$PATH:"/cygdrive/c/Program Files (x86)/Mono/bin"
PATH="$PREFIX/bin:$PATH"
git clone git://github.com/mono/mono.git mono32
cd mono32
CFLAGS=-static-libgcc ./autogen.sh --prefix=$PREFIX --host=i686-w64-mingw32
make
make install
```

Mono 64-bit build:
``` bash
PATH=$PATH:"/cygdrive/c/Program Files (x86)/Mono/bin"
PATH="$PREFIX/bin:$PATH"
git clone git://github.com/mono/mono.git mono64
cd mono64
CFLAGS=-static-libgcc ./autogen.sh --prefix=$PREFIX --host=x86_64-w64-mingw32
make
make install
```

See also these articles:

 - [Compiling with Visual Studio](/docs/compiling-mono/windows/compiling-with-visualstudio/)
 - [Cross-compiling Mono for Windows](/docs/compiling-mono/windows/cross-compiling-mono-for-windows/)
