#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

dnl AC_PREREQ([2.63])
AC_INIT([qmidiroute], [0.4.0], [alsamodular-devel@lists.sourceforge.net])
AC_CONFIG_SRCDIR([src/main.cpp])
AC_CONFIG_HEADERS(src/config.h)
AM_INIT_AUTOMAKE([dist-bzip2])
AC_DEFINE(APP_NAME, "QMidiRoute", [application name])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_LN_S

AC_LANG_CPLUSPLUS 

dnl option to enable Qt5 instead of default Qt4 library 
AC_ARG_ENABLE(qt5, AC_HELP_STRING([--enable-qt5],
                          [enable Qt5 instead of Qt4 library]),,
                          enable_qt4=yes)
AS_IF([test x$enable_qt4 = xyes],
dnl Check for Qt4 library files
[
PKG_CHECK_MODULES(Qt4, QtCore QtGui QtNetwork >= 4.2.0,
                  HAVE_QT4="yes", HAVE_QT4="no")
if test "x$HAVE_QT4" = "xyes"; then
  dnl Check needed because in some cases the QtGui includedir
  dnl doesn't contain the subsystem dir.
  QT_INCLUDE_DIR=`$PKG_CONFIG --variable=includedir QtGui`
  QT_CXXFLAGS=`$PKG_CONFIG --cflags QtCore QtGui QtOpenGL`
  EXTRA_QT_INCLUDE_DIR="$QT_INCLUDE_DIR/Qt"
  AC_CHECK_FILE([$QT_INCLUDE_DIR/QWidget],
    AC_MSG_NOTICE([No extra QT_INCLUDE_DIR needed]),
      AC_CHECK_FILE([$EXTRA_QT_INCLUDE_DIR/QWidget],
        QT_CPPFLAGS="$QT_CPPFLAGS -I$EXTRA_QT_INCLUDE_DIR",
          AC_MSG_WARN([QWidget not found])))
    
  AC_MSG_NOTICE([Set QT_CXXFLAGS... $QT_CXXFLAGS])
  AC_SUBST(QT_CXXFLAGS)
  AC_DEFINE([HAVE_QT4], [1],[Define to 1 if you have Qt4.])

  AC_CHECK_LIB([QtCore], [main], ,
             [AC_MSG_ERROR(QtCore is required)])
  AC_CHECK_LIB([QtGui], [main], ,
             [AC_MSG_ERROR(QtGui is required)])

else
  AC_MSG_ERROR([cannot find Qt4 library >= 4.2])
fi
ac_qtversion=qt4],
dnl ... or use Qt5
[
PKG_CHECK_MODULES(Qt5, Qt5Core Qt5Gui Qt5Widgets >= 5.0,
                  HAVE_QT5="yes", HAVE_QT5="no")
if test "x$HAVE_QT5" = "xyes"; then
  dnl Check needed because in some cases the QtGui includedir
  dnl doesn't contain the subsystem dir.
  QT_INCLUDE_DIR=`$PKG_CONFIG --variable=includedir Qt5Gui`
  QT_CXXFLAGS=`$PKG_CONFIG --cflags Qt5Core Qt5Gui Qt5OpenGL Qt5Widgets`
  QT_CXXFLAGS="$QT_CXXFLAGS -fPIC"
  EXTRA_QT_INCLUDE_DIR="$QT_INCLUDE_DIR/Qt"
  AC_CHECK_FILE([$QT_INCLUDE_DIR/QtWidgets/QWidget],
    AC_MSG_NOTICE([No extra QT_INCLUDE_DIR needed]),
      AC_CHECK_FILE([$EXTRA_QT_INCLUDE_DIR/QtWidgets/QWidget],
        QT_CPPFLAGS="$QT_CPPFLAGS -I$EXTRA_QT_INCLUDE_DIR",
          AC_MSG_WARN([QWidget not found])))
    
  AC_MSG_NOTICE([Set QT_CXXFLAGS... $QT_CXXFLAGS])
  AC_SUBST(QT_CXXFLAGS)
  AC_DEFINE([HAVE_QT5], [1],[Define to 1 if you have Qt5.])

AC_CHECK_LIB([Qt5Core], [main], ,
             [AC_MSG_ERROR(QtCore is required)])
AC_CHECK_LIB([Qt5Gui], [main], ,
             [AC_MSG_ERROR(QtGui is required)])
AC_CHECK_LIB([Qt5Widgets], [main], ,
             [AC_MSG_ERROR(QtWidget is required)])

else
  AC_MSG_ERROR([cannot find Qt5 library >= 5.0])
fi
ac_qtversion=qt5
])
dnl endo of qt version test

dnl check for moc
AC_PATH_PROGS(MOC, moc-$ac_qtversion moc, no, [$PATH:$QTDIR/bin])
if test "x$MOC" = "xno" ; then
  AC_MSG_ERROR(
    [cannot find 'moc', please install the Qt development tools package.])
fi

dnl check for lupdate
AC_PATH_PROGS(LUPDATE, lupdate-$ac_qtversion lupdate, no, [$PATH:$QTDIR/bin])
if test "x$LUPDATE" = "xno" ; then
  AC_MSG_ERROR(
    [cannot find 'lupdate', please install the Qt development tools package.])
fi

dnl check for lrelease
AC_PATH_PROGS(LRELEASE, lrelease-$ac_qtversion lrelease, no, [$PATH:$QTDIR/bin])
if test "x$LRELEASE" = "xno" ; then
  AC_MSG_ERROR(
    [cannot find 'lrelease', please install the Qt development tools package.])
fi


# Checks for other libraries.
AC_CHECK_LIB([asound], [snd_pcm_open], ,
             [AC_MSG_ERROR(ALSA library (asound) is required)])

# Checks for header files.
dnl AC_HEADER_STDC
AC_CHECK_HEADERS([cstdio])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_TYPE_SSIZE_T
AC_STRUCT_TM

# Checks for library functions.
dnl AC_FUNC_MALLOC
dnl AC_FUNC_STAT
dnl AC_CHECK_FUNCS([strdup])

AC_CONFIG_FILES([Makefile] [examples/Makefile] [src/Makefile]
    [src/pixmaps/Makefile] [man/Makefile] [man/fr/Makefile] [man/de/Makefile])
AC_OUTPUT
