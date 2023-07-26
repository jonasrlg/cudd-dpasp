# CUDD

CUDD version 3.0.0 from  David Kebo Tutorials

The CUDD package is a package written in C for the manipulation of
decision diagrams.  It supports binary decision diagrams (BDDs),
algebraic decision diagrams (ADDs), and Zero-Suppressed BDDs (ZDDs).

This directory contains a set of packages that allow you to build a test
application based on the CUDD package.

The test application provided in this kit is called nanotrav and is a
simple-minded FSM traversal program.  (See the README file and the man
page nanotrav.1 in the nanotrav directory for the details.)  It is
included so that you can run a sanity check on your installation.

Also included in this distribution are the dddmp libray by Giampiero
Cabodi and Stefano Quer and a C++ object-oriented wrapper for CUDD.

# Documentatiom

The user's manual in PDF format can be found in `CUDD_User_Manual.pdf`; and is built in `/doc` if `pdflatex` and `makeindex` are installed.

If `doxygen` is installed, running `make` puts HTML documentation for
the CUDD package in directory `/cudd/html`. The recommended
starting point is index.html. Documentation for the `dddmp` library is in the `dddmp/doc` subdirectory.

# Build and Installations

## Dependencies

- `autotools` (`autoconf` and `automake`on Fedora)
- `g++` (`gcc-c++` on Fedora).

## Installation

In the simplest form, you can build the static libraries with:

```sh
  autoreconf
  ./configure --prefix=/usr --enable-shared --enable-dddmp --enable-obj
  make
  make check
  sudo make install
  sudo install -Dm644 util/util.h -t /usr/include/cudd/
  sudo install -Dm644 config.h -t /usr/include/cudd/
  sudo install -Dm644 mtr/mtr.h -t /usr/include/cudd/
  sudo install -Dm644 LICENSE -t /usr/share/licenses/cudd/
```

The configure script provides a few options, which can be listed with

```sh
    ./configure --help
```

Notable options include

```sh
  --enable-silent-rules
  --enable-shared
  --enable-dddmp
  --enable-obj
  --with-system-qsort
```

The `--enable-silent-rules` option is a standard option that streamlines the
messages produced by the build process.  The remaining options are specific
to CUDD.

The three `enable` options control the build of shared libraries.  By
default, only static libraries are built.  With `--enable-shared`, a
shared library for libcudd is built.  (Before installation, it can be
found in `cudd/.libs`.)

The last two "enable" options control the inclusion of the dddmp
library and C++ wrapper in the shared library, which by default only
contains the core CUDD library.

The `--with-system-qsort` option requests use of the `qsort` from the
standard library instead of the portable one shipped with CUDD.  This
option is provided for backward compatibility and is not otherwise
recommended.  Some of the tests of `make check` may fail with the
system `qsort` because variable orders may be generated that are
different from the reference ones.

As an example, a more elaborate build command sequence may be:

```sh
  ./configure CC=clang CXX=clang++ --enable-silent-rules \
    --enable-shared --enable-obj
  make -j4 check
  make install
```

which selects alternate compilers instead of `gcc` and `g++`, causes the
C++ wrapper to be included in the shared library, enables parallel
compilation (with `-j4`) and finally installs the shared library using
the default prefix `/usr/local`.

For those unfamiliar with `libtool` it may be worth noting that the
libraries it builds go in `.libs` subdirectories.  One should also note
that with shared libraries enabled, the test programs immediately
visible to the user are shell scripts that make sure dynamic linking
works before installation.  If you want to run valgrind on, say, a
dynamically linked nanotrav, specify the option `--trace-children=yes`.

# Plataforms

This kit has been successfully built on the following configurations:

    PC (x86 and x86_64) running Ubuntu with gcc and clang
    PC (x86 and x86_64) running Ubuntu with g++
    PC (x86 and x86_64) running Linux RedHat with gcc
    PC (x86 and x86_64) running Linux RedHat with g++
    PC (x86_64) running 32-bit Cygwin on Windows 7 and Vista with gcc
    PC (x86_64) running 32-bit Cygwin on Windows 7 and Vista with g++
    PC (x86_64) running 64-bit Cygwin on Windows 8.1 with gcc and g++
    PC (x86_64) running MinGW-w64 on Windows 8.1 with gcc

In all these cases, the C++ wrapper was compiled with the matching C++
compiler (`g++` for `gcc` and `clang++` for `clang`).  To compile under MSYS2
(MinGW-w64) one has to pass `--build=x86_64-w64-mingw32` to `./configure`.

# Sanity Check

The directory `nanotrav` contains a simple application based on the
CUDD package.  The `nanotrav` directory contains a man page that
describes the options `nanotrav` supports.  The files `*.blif` are sample
input files for `nanotrav`. The `*.out` files are the reference output
files.

# Troubleshooting

In case of `aclocal-1.14 is missing on your system` problem while running `./configure`, run `autoreconf` befora running `./configure` again.

# Feedback

Send feedback to:

```
Fabio Somenzi
University of Colorado at Boulder
ECE Dept.
Boulder, CO 80309-0425
Fabio@Colorado.EDU
https://www.colorado.edu/ecee/fabio-somenzi
```
