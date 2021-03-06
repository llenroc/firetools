AC_PREREQ([2.68])
AC_INIT(firetools, 0.9.30, netblue30@yahoo.com, , http://firejail.sourceforge.net)
AC_CONFIG_SRCDIR([src/firetools/main.cpp])
#AC_CONFIG_HEADERS([config.h])


AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_RANLIB

# checking pthread library
AC_CHECK_LIB([pthread], [main], [], AC_MSG_ERROR([*** POSIX thread support not installed - please install first ***]))
AC_CHECK_HEADER(pthread.h,,AC_MSG_ERROR([*** POSIX thread support not installed - please install first ***]))

# checking qt4/qt5 library
if test -f /usr/lib/x86_64-linux-gnu/libQt5Core.so
then
	echo "Found qt5 library in /usr/lib/x86_64-linux-gnu directory"
	# check libqt5svg5 library
	if test -f /usr/lib/x86_64-linux-gnu/qt5/plugins/imageformats/libqsvg.so
	then
		echo "Found qt5 svg library"
	else
		AC_MSG_ERROR([*** Please install Qt5 svg support - apt-get install libqt5svg5 ***])
	fi
	if test -f /usr/lib/x86_64-linux-gnu/qt5/plugins/iconengines/libqsvgicon.so
	then
		echo "Found qt5 svg icon library"
	else
		AC_MSG_ERROR([*** Please install Qt5 svg support - apt-get install libqt5svg5 ***])
	fi

# Arch Linux 64bit
elif test -f /usr/lib/libQt5Core.so
then
        echo "Found qt5 library in /usr/lib directory"
        # check libqt5svg5 library
        if test -f /usr/lib/qt/plugins/imageformats/libqsvg.so
        then
                echo "Found qt5 svg library"
        else
                AC_MSG_ERROR([*** Please install Qt5 svg support - pacman -S qt5-svg ***])
        fi
        if test -f /usr/lib/qt/plugins/iconengines/libqsvgicon.so
        then
                echo "Found qt5 svg icon library"
        else
                AC_MSG_ERROR([*** Please install Qt5 svg support - pacman -S qt5-svg ***])
        fi
	
# Gentoo 64bit
elif test -f /usr/lib64/qt4/libQtCore.so
then
	echo "Found qt4 library in /usr/lib64/qt4 directory"

# Gentoo 32bit
elif test -f /usr/lib/qt4/libQtCore.so
then
	echo "Found qt4 library in /usr/lib/qt4 directory"
else
	AC_CHECK_LIB([QtCore], [main], [], AC_MSG_ERROR([*** QtCore library not installed - please install Qt4 or Qt5 first ***]))
	AC_CHECK_LIB([QtGui], [main], [], AC_MSG_ERROR([*** QtCore library not installed - please install Qt4 or Qt5 first ***]))

fi

# testing qmake
QMAKE=`which qmake`
if test -z "$QMAKE"
then
	# fix for Fedora
	if test -f /usr/lib64/qt4/bin/qmake; then
		QMAKE=/usr/lib64/qt4/bin/qmake
	fi
	
	if test -z "$QMAKE"
	then
		AC_MSG_ERROR([qmake is missing, please install Qt4 or Qt5 development packages.])
	fi
fi
echo "Found $QMAKE"
$QMAKE -v

cd src/firetools && $QMAKE && cd ../..

# build sanitizer.sh file
echo "#!/bin/bash" >sanitizer.sh
echo "export CFLAGS=-fsanitize=address" >> sanitizer.sh
echo "export CXXFLAGS=-fsanitize=address" >> sanitizer.sh
echo "export LDFLAGS=-lasan" >> sanitizer.sh
echo "cd src/firetools && $QMAKE && cd ../.." >> sanitizer.sh
chmod +x sanitizer.sh

# testing firejail
FIREJAIL=`which firejail`
if test -z "$FIREJAIL"
then
	AC_MSG_WARN([Firejail is required for running firetools, please install it (http://firejail.sf.net/).])
else
	echo "Found $FIREJAIL"
fi

# testing xterm
XTERM=`which xterm`
if test -z "$XTERM"
then
	AC_MSG_WARN([xterm is required for running firetools, please install it.])
else
	echo "Found $XTERM"
fi

cp confdefs.h firetools_config.h
AC_OUTPUT(Makefile)

