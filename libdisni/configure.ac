#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

#AC_PREREQ([2.67])
AC_INIT([libdisni], [1.1], [https://github.com/zrlio/disni/issues])
AC_CONFIG_SRCDIR([src/verbs/com_ibm_disni_rdma_verbs_impl_NativeDispatcher.cpp])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIRS([m4])

AM_INIT_AUTOMAKE([foreign])
LT_INIT([disable-static])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_CPP

# Checks for libraries.
AC_CHECK_LIB([ibverbs], [ibv_get_device_list], [],
	     [AC_MSG_ERROR(disni requires libibverbs)])
AC_CHECK_LIB([rdmacm], [rdma_create_id], [],
	     [AC_MSG_ERROR(disni requires librdmacm)])

AC_ARG_WITH([jdk],
            [AS_HELP_STRING([--with-jdk],[jdk base directory])],
            [jdk=$withval],
            [AC_MSG_ERROR(jdk directory not specified)])
AC_SUBST([jdk])

AC_ARG_WITH([spdk],
            [AS_HELP_STRING([--with-spdk],[spdk base directory])],
            [spdk=$withval])
AC_SUBST([spdk])
AM_CONDITIONAL([SPDK], [test "x$with_spdk" != "x"])

AC_ARG_WITH([dpdk],
            [AS_HELP_STRING([--with-dpdk],[dpdk base directory])],
            [dpdk=$withval],
            AS_IF([test "x$with_spdk" != "x"],[AC_MSG_ERROR(dpdk directory not specified)]))
AC_SUBST([dpdk])

AC_ARG_ENABLE(logging, [  --enable-logging],
[if test "$enableval" = yes; then
  AC_DEFINE([ENABLE_LOGGING], , [Enable logging])
fi], [enable_logging=no])

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h netinet/in.h stdlib.h string.h sys/socket.h rdma/rdma_cma.h],
		 [], [AC_MSG_ERROR(required header file missing)])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_UINT32_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([memset])

AC_CONFIG_FILES([Makefile
                 src/Makefile
		 src/verbs/Makefile
		 src/affinity/Makefile
         src/nvmef/Makefile])
AC_OUTPUT

AS_ECHO(["
-----------------------------------------------------------------------
${PACKAGE_NAME} Version ${PACKAGE_VERSION}

Using installation prefix: ${prefix}
Using installation exec_prefix: ${exec_prefix}
Using libdir: ${libdir}
Using C++ compiler: ${CXX} ${CXXFLAGS} ${CPPFLAGS}
jdk base directory: ${jdk}
SPDK base directory: ${spdk}
DPDK base directory: ${dpdk}
Enable logging: ${enable_logging}

You can now run 'make' to build and 'make install' to install.
-----------------------------------------------------------------------
"])
