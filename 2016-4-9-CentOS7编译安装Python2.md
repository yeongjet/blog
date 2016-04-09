1.下载
`wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz`

2.创建编译输出目录
`mkdir /usr/local/python27`

3.编译环境
`./configure --prefix=/usr/local/python27`

4.编译
`make`

5.安装
`make install`

6.`sudo mv /usr/bin/python /usr/bin/python-2.7.5`
但是我发现这步要在安装前进行,否则依旧是老版本

7.`ln -s /usr/local/python27/bin/python /usr/bin/python`

8.分别执行`python -V`和`python-2.7.5 -V`
分别显示的是`Python 2.7.5`和`Python 2.7.11`

9.`vi /usr/bin/yum`
将第一行中的`#!/usr/bin/python`修改为`#!/usr/bin/python-2.7.5`


过程中提示
INFO: Can't locate Tcl/Tk libs and/or headers
Python build finished, but the necessary bits to build these modules were not found:
_bsddb             _curses            _curses_panel   
_sqlite3           _ssl               _tkinter        
bsddb185           bz2                dbm             
dl                 gdbm               imageop         
readline           sunaudiodev        zlib            
To find the necessary bits, look in setup.py in detect_modules() for the module's name.

附编译选项
`configure' configures python 2.7 to adapt to many kinds of systems.

Usage: ./configure [OPTION]... [VAR=VALUE]...

To assign environment variables (e.g., CC, CFLAGS...), specify them as
VAR=VALUE.  See below for descriptions of some of the useful variables.

Defaults for the options are specified in brackets.

Configuration:
  -h, --help              display this help and exit
      --help=short        display options specific to this package
      --help=recursive    display the short help of all the included packages
  -V, --version           display version information and exit
  -q, --quiet, --silent   do not print `checking ...' messages
      --cache-file=FILE   cache test results in FILE [disabled]
  -C, --config-cache      alias for `--cache-file=config.cache'
  -n, --no-create         do not create output files
      --srcdir=DIR        find the sources in DIR [configure dir or `..']

Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
  --exec-prefix=EPREFIX   install architecture-dependent files in EPREFIX
                          [PREFIX]

By default, `make install' will install all the files in
`/usr/local/bin', `/usr/local/lib' etc.  You can specify
an installation prefix other than `/usr/local' using `--prefix',
for instance `--prefix=$HOME'.

For better control, use the options below.

Fine tuning of the installation directories:
  --bindir=DIR            user executables [EPREFIX/bin]
  --sbindir=DIR           system admin executables [EPREFIX/sbin]
  --libexecdir=DIR        program executables [EPREFIX/libexec]
  --sysconfdir=DIR        read-only single-machine data [PREFIX/etc]
  --sharedstatedir=DIR    modifiable architecture-independent data [PREFIX/com]
  --localstatedir=DIR     modifiable single-machine data [PREFIX/var]
  --libdir=DIR            object code libraries [EPREFIX/lib]
  --includedir=DIR        C header files [PREFIX/include]
  --oldincludedir=DIR     C header files for non-gcc [/usr/include]
  --datarootdir=DIR       read-only arch.-independent data root [PREFIX/share]
  --datadir=DIR           read-only architecture-independent data [DATAROOTDIR]
  --infodir=DIR           info documentation [DATAROOTDIR/info]
  --localedir=DIR         locale-dependent data [DATAROOTDIR/locale]
  --mandir=DIR            man documentation [DATAROOTDIR/man]
  --docdir=DIR            documentation root [DATAROOTDIR/doc/python]
  --htmldir=DIR           html documentation [DOCDIR]
  --dvidir=DIR            dvi documentation [DOCDIR]
  --pdfdir=DIR            pdf documentation [DOCDIR]
  --psdir=DIR             ps documentation [DOCDIR]

System types:
  --build=BUILD     configure for building on BUILD [guessed]
  --host=HOST       cross-compile to build programs to run on HOST [BUILD]

Optional Features:
  --disable-option-checking  ignore unrecognized --enable/--with options
  --disable-FEATURE       do not include FEATURE (same as --enable-FEATURE=no)
  --enable-FEATURE[=ARG]  include FEATURE [ARG=yes]
  --enable-universalsdk[=SDKDIR]
                          Build against Mac OS X 10.4u SDK (ppc/i386)
  --enable-framework[=INSTALLDIR]
                          Build (MacOSX|Darwin) framework
  --enable-shared         disable/enable building shared python library
  --enable-profiling      enable C-level code profiling
  --enable-toolbox-glue   disable/enable MacOSX glue code for extensions
  --enable-ipv6           Enable ipv6 (with ipv4) support
  --disable-ipv6          Disable ipv6 support
  --enable-big-digits[=BITS]
                          use big digits for Python longs [[BITS=30]]
  --enable-unicode[=ucs[24]]
                          Enable Unicode strings (default is ucs2)

Optional Packages:
  --with-PACKAGE[=ARG]    use PACKAGE [ARG=yes]
  --without-PACKAGE       do not use PACKAGE (same as --with-PACKAGE=no)
  --with-universal-archs=ARCH
                          select architectures for universal build ("32-bit",
                          "64-bit", "3-way", "intel" or "all")
  --with-framework-name=FRAMEWORK
                          specify an alternate name of the framework built
                          with --enable-framework
  --without-gcc           never use gcc
  --with-cxx-main=<compiler>
                          compile main() and link python executable with C++
                          compiler
  --with-suffix=.exe      set executable suffix
  --with-pydebug          build with Py_DEBUG defined
  --with-libs='lib1 ...'  link against additional libs
  --with-system-expat     build pyexpat module using an installed expat
                          library
  --with-system-ffi       build _ctypes module using an installed ffi library
  --with-tcltk-includes='-I...'
                          override search for Tcl and Tk include files
  --with-tcltk-libs='-L...'
                          override search for Tcl and Tk libs
  --with-dbmliborder=db1:db2:...
                          order to check db backends for dbm. Valid value is a
                          colon separated string with the backend names
                          `ndbm', `gdbm' and `bdb'.
  --with-signal-module    disable/enable signal module
  --with-dec-threads      use DEC Alpha/OSF1 thread-safe libraries
  --with(out)-threads[=DIRECTORY]
                          disable/enable thread support
  --with(out)-thread[=DIRECTORY]
                          deprecated; use --with(out)-threads
  --with-pth              use GNU pth threading libraries
  --with(out)-doc-strings disable/enable documentation strings
  --with(out)-tsc         enable/disable timestamp counter profile
  --with(out)-pymalloc    disable/enable specialized mallocs
  --with-valgrind         Enable Valgrind support
  --with-wctype-functions use wctype.h functions
  --with-fpectl           enable SIGFPE catching
  --with-libm=STRING      math library
  --with-libc=STRING      C library
  --with(out)-computed-gotos
                          Use computed gotos in evaluation loop (enabled by
                          default on supported compilers)
  --with(out)-ensurepip=[=OPTION]
                          "install" or "upgrade" using bundled pip, default is
                          "no"

Some influential environment variables:
  CC          C compiler command
  CFLAGS      C compiler flags
  LDFLAGS     linker flags, e.g. -L<lib dir> if you have libraries in a
              nonstandard directory <lib dir>
  LIBS        libraries to pass to the linker, e.g. -l<library>
  CPPFLAGS    (Objective) C/C++ preprocessor flags, e.g. -I<include dir> if
              you have headers in a nonstandard directory <include dir>
  CPP         C preprocessor
  PKG_CONFIG  path to pkg-config utility
  PKG_CONFIG_PATH
              directories to add to pkg-config's search path
  PKG_CONFIG_LIBDIR
              path overriding pkg-config's built-in search path

Use these variables to override the choices made by `configure' or to help
it to find libraries and programs with nonstandard names/locations.

Report bugs to <http://bugs.python.org/>.
