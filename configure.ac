#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.64)
AC_INIT([idevicerestore], [1.0], [https://github.com/s0uthwest/idevicerestore/issues])

AC_CANONICAL_SYSTEM
AM_INIT_AUTOMAKE([dist-bzip2 no-dist-gzip])

m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES])

AC_CONFIG_SRCDIR([src/])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
AC_PROG_LIBTOOL

# Checks for libraries.
AC_SEARCH_LIBS([pthread_create], [pthread])
PKG_CHECK_MODULES(libirecovery, libirecovery >= 0.2.0)
PKG_CHECK_MODULES(libimobiledevice, libimobiledevice-1.0 >= 1.2.1)
PKG_CHECK_MODULES(libplist, libplist >= 1.0)
PKG_CHECK_MODULES(libzip, libzip >= 1.4)
PKG_CHECK_MODULES(libusbmuxd, libusbmuxd >= 1.0)
PKG_CHECK_MODULES(libcurl, libcurl >= 7.0)
PKG_CHECK_MODULES(openssl, openssl >= 0.9.8)
PKG_CHECK_MODULES(zlib, zlib >= 1.2)

GLOBAL_CFLAGS=""
AC_LDADD=""
AC_LDFLAGS=""
case "$host_os" in
	darwin*)
	;;
	linux*)
		GLOBAL_CFLAGS+="-lpthread"
	;;
	mingw*)
		GLOBAL_CFLAGS+="-DWIN32 -D__LITTLE_ENDIAN__=1"
		AC_LDFLAGS+="-static-libgcc"
	;;
	*)
	;;
esac

AC_CHECK_FUNCS([strsep strcspn])
if test x$ac_cv_func_strsep != xyes; then
  if test x$ac_cv_func_strcspn != xyes; then
    AC_MSG_ERROR([You need either strsep or strcspn to build $PACKAGE])
  fi
fi

AC_SUBST(GLOBAL_CFLAGS)
AC_SUBST(AC_LDFLAGS)
AC_SUBST(AC_LDADD)

case $CFLAGS in
  *IDEVICERESTORE_NOMAIN*)
    nomain=true
  ;;
  *)
  ;;
esac
AM_CONDITIONAL(NOMAIN, test x$nomain == xtrue)

AC_MSG_CHECKING([whether to enable WIN32 specific flags])
case "$host_os" in
*mingw*)
        win32=true
        AC_MSG_RESULT([yes])
	AC_CHECK_TOOL(WINDRES, windres)
  ;;
*)
        win32=false
        AC_MSG_RESULT([no])
  ;;
esac
AM_CONDITIONAL(WIN32, test x$win32 = xtrue)

# check for large file support
AC_SYS_LARGEFILE
LFS_CFLAGS=''
if test "$enable_largefile" != no; then
	if test "$ac_cv_sys_file_offset_bits" != 'no'; then
		LFS_CFLAGS="$LFS_CFLAGS -D_FILE_OFFSET_BITS=$ac_cv_sys_file_offset_bits"
	else
		AC_MSG_CHECKING(for native large file support)
		AC_RUN_IFELSE([AC_LANG_SOURCE([#include <unistd.h>
		  int main (int argc, char **argv)
		  {
		      exit(!(sizeof(off_t) == 8));
		  }])],
		[ac_cv_sys_file_offset_bits=64; AC_DEFINE(_FILE_OFFSET_BITS,64)
		 AC_MSG_RESULT(yes)],
		[AC_MSG_RESULT(no)])
	fi
	if test "$ac_cv_sys_large_files" != 'no'; then
		LFS_CFLAGS="$LFS_CFLAGS -D_LARGE_FILES=1"
	fi
	AC_FUNC_FSEEKO
	if test "$ac_cv_sys_largefile_source" != 'no'; then
		LFS_CFLAGS="$LFS_CFLAGS -D_LARGEFILE_SOURCE=1"
	fi
fi
AC_SUBST(LFS_CFLAGS)

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AC_OUTPUT([
Makefile
src/Makefile
docs/Makefile
])

echo "
Configuration for $PACKAGE $VERSION:
-------------------------------------------

  Install prefix: .........: $prefix

  Now type 'make' to build $PACKAGE $VERSION,
  and then 'make install' for installation.
"
