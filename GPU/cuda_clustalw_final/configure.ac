# Andreas Wilm (UCD): 2007-12-10
# Generated using the following sites 
# http://www.openismus.com/documents/linux/automake/automake.shtml
# http://www.bioinf.uni-freiburg.de/~mmann/HowTo/automake.html

# autoconf looks for a file called configure.ac (or, previously,
# configure.in). It then creates the configure script, based on the
# macros which it finds.  Whenever you add a macro to configure.ac,
# you should run aclocal as well as autoconf, because aclocal scans
# configure.ac to find which macros it should provide.


# Andreas Wilm (UCD):
#  you will only have to change the version number here!
#  after apllying any changes you will have to run
#  $ bash auto.sh
#
AC_INIT([clustalw], [2.0.11], [clustalw@ucd.ie])
#
# The AC_INIT macro can take any source file as an argument. It just
# checks that the file is there, which should, in turn, mean that the
# source directory is there.
# AC_INIT generates the following m4 macros, output variables and preprocessor symbols:
#  * [AC_]PACKAGE_NAME
#  * [AC_]PACKAGE_TARNAME
#  * [AC_]PACKAGE_VERSION
#  * [AC_]PACKAGE_STRING
#  * [AC_]PACKAGE_BUGREPORT 


# Detect the canonical host and target build environment
# see e.g. http://cvs.haskell.org/darcs/ghc/configure.ac for howto use
# AC_CANONICAL_HOST or AC_CANONICAL_TARGET?
# AC_CANONICAL_SYSTEM will also run AC_CANONICAL_HOST and allows to use --target
AC_CANONICAL_SYSTEM
# DEFINES here are going to config.h and have to be the same as in clustalx.pro
#echo "host_os $host_os"
case $target_os in
   *linux*|*solaris*|*bsd*)
        AC_DEFINE([OS_UNIX], [1], [Host OS])
        ;; 
   *darwin*)
        AC_DEFINE([OS_MAC], [1], [Host OS])
        AC_DEFINE([OS_UNIX], [1], [Host OS])
        ;; 
   *cygwin*|*mingw*)
        AC_DEFINE([OS_WINDOWS], [1], [Host OS])
        ;;
   *)
        AC_MSG_WARN(["Couldn't determine target operating system. Using Unix"]);
        AC_DEFINE([OS_UNIX], [1], [Host OS])
        ;;
esac
echo "building for $target_os"


# set in clustalw_version.h[.in] rather than config.h to avoid name clashes
#AC_DEFINE_UNQUOTED(CLUSTALW_VERSION, "${PACKAGE_VERSION}", [ClustalW version])
#AC_DEFINE_UNQUOTED(CLUSTALW_NAME, "ClustalW", [ClustalW name])
CLUSTALW_VERSION="${PACKAGE_VERSION}"
AC_SUBST(CLUSTALW_VERSION)
CLUSTALW_NAME="ClustalW"
AC_SUBST(CLUSTALW_NAME)

# The AM_INIT_AUTOMAKE line adds several standard checks. 
# This macro is always needed for automake
# Obsolete: It takes the program name and version number as arguments.
AM_INIT_AUTOMAKE()

AC_PROG_MAKE_SET




# AC_PROG_CC indicates that the source code may be in C. If the source
# code is C++ then we also need AC_PROG_CXX.



AC_PROG_CC
AC_PROG_CXX

# --- OpenMP (optional for multi threading) ---
# -- AC_OPENMP macro is new in autoconf 2.62 --
#AC_OPENMP
#CXXFLAGS="$OPENMP_CXXFLAGS $CXXFLAGS"
#AC_SUBST(OPENMP_CXXFLAGS)
AC_ARG_WITH(OpenMP,[ --without-OpenMP disable OpenMP (Multi-Processing)],
[ac_cv_use_openmp=$withval], [ac_cv_use_openmp=yes])
if test x$ac_cv_use_openmp != xno; then
AC_CHECK_LIB(gomp,GOMP_parallel_start,,AC_SUBST(noopenmp,1))
else
AC_SUBST(noopenmp, 1)
fi
if [[ "$noopenmp" != "1" ]]; then
LDFLAGS="$LDFLAGS -lgomp"
CXXFLAGS="$CXXFLAGS -fopenmp"
fi
AM_CONDITIONAL([noopenmp], [test "$noopenmp" = 1])


########################
# Check for CUDA
########################
AC_ARG_WITH([cuda],
    [  --with-cuda=PATH        prefix where cuda is installed [default=auto]])
if test "x$with_cuda" != x; then
    CUDA_CFLAGS="-I$with_cuda/include"
    CUDA_LIBS="-L$with_cuda/lib -lcuda -lcudart"
    NVCC="$with_cuda/bin/nvcc"
else
    CUDA_CFLAGS="-I/usr/local/cuda/include"
    CUDA_LIBS="-L/usr/local/cuda/lib64 -lcuda -lcudart "
    NVCC="nvcc"
fi
NVCCFLAGS="-arch=sm_13 -use_fast_math  -Xptxas -v -Xcompiler -fopenmp "
AC_SUBST(CUDA_CFLAGS)
AC_SUBST(CUDA_LIBS)
AC_SUBST(NVCC)
AC_SUBST(NVCCFLAGS)





# AC_PROG_RANLIB will allow you to build code in sub-directories into
# temporary libraries, which make will then link in with the rest of
# the code.
AC_PROG_RANLIB
# AC_PROG_INSTALL will generate an install target so that users may
# just type 'make install' to install the software.
AC_PROG_INSTALL


# Checks for header files.
AC_HEADER_STDC
# AC_CHECK_HEADERS([a.h b.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_TYPE_SIZE_T



# This may beincluded by your source code and provides a way for
# people to customise the configuration for their platform, via
# #defines. config.h.in can be generated automatically with the
# autoheader tool.  However, you need a stamp-h file in your project
# to ensure that automake regenerates config.h from config.h.in. Type
# 'touch stamp-h' to add this file to your project.
# Andreas Wilm: standard would be config.h but let's avoid conflicts with 
AM_CONFIG_HEADER(src/config.h) 

AC_FUNC_MALLOC

# enable hashing if requested (--with-mhash)
# will set HAVE_MHASH_H only 
AC_ARG_WITH([mhash],
	[AS_HELP_STRING([--with-mhash],
       [Enable mhash support in stats])],
       [with_mhash=$withval],
       [with_mhash=no])
if test "$with_mhash" = "yes"; then
    AC_CHECK_LIB([mhash], [mhash_init], ,
       [AC_MSG_ERROR([mhash lib not found])])
    AC_CHECK_HEADERS([mhash.h], ,
       [AC_MSG_ERROR([mhash header not found])])

fi
        

# AC_OUTPUT indicates the name of the Makefile which will be generated.
### AC_CONFIG_FILES([ Makefile ])
AC_OUTPUT(Makefile m4/Makefile src/Makefile src/clustalw_version.h)














