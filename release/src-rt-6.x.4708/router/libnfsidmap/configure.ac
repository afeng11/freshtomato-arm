#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.66])
AC_INIT([libnfsidmap],[0.27],[linux-nfs@vger.kernel.org])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_SRCDIR([nfsidmap.h])
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([foreign])
LT_INIT

# Checks for programs.
AC_PROG_CC

# Checks for libraries.

AC_CHECK_LIB([resolv], [__res_querydomain], , AC_MSG_ERROR(res_querydomain needed))

AC_ARG_ENABLE([ldap],
	[AS_HELP_STRING([--disable-ldap],[Disable support for LDAP @<:@default=detect@:>@])])
if test "x$enable_ldap" != "xno" ; then
	AC_CHECK_HEADER([ldap.h],
		[AC_CHECK_LIB([ldap], [ldap_initialize],
		              [have_ldap="yes"],[have_ldap="no"])],
		[have_ldap="no"])
	if test "x$have_ldap" = "xyes" ; then
		AC_DEFINE([ENABLE_LDAP], 1, [Enable LDAP Support])
	elif test "x$enable_ldap$have_ldap" = "xyesno" ; then
		AC_MSG_ERROR(LDAP support not found!)
	fi
fi
AM_CONDITIONAL(ENABLE_LDAP, test "x$have_ldap" = "xyes")

# Should we build gums mapping library?
AC_ARG_ENABLE([gums],
	[AS_HELP_STRING([--enable-gums],[Enable support for the GUMS mapping library @<:@default=false@:>@])])
if test "x$enable_gums" = "xyes" ; then
	AC_DEFINE([ENABLE_GUMS], 1, [Enable GUMS mapping library support])
fi
AM_CONDITIONAL(ENABLE_GUMS, test "x$enable_gums" = "xyes")

#Where do the Plugins live
AC_ARG_WITH(pluginpath,
    [AS_HELP_STRING([--with-pluginpath=/foo],[Causes the library to look in /foo instead of /usr/lib/libnfsidmap for plugins
    ])],
    path_plugins=$withval,
    path_plugins=""
    )
if test -n "$path_plugins" ; then
	AC_DEFINE_UNQUOTED(PATH_PLUGINS, "$path_plugins",
		[Define this to change the plugins path])
fi
AM_CONDITIONAL(PATH_PLUGINS, test -n "$path_plugins")

AC_CONFIG_HEADERS([./config.h])
# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([stdlib.h string.h unistd.h errno.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_UID_T
AC_TYPE_SIZE_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([strchr strdup])

AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([libnfsidmap.pc])
AC_OUTPUT
