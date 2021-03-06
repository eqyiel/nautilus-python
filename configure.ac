AC_INIT([nautilus-python], [1.2.2])

AM_CONFIG_HEADER(config.h)
AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE

AC_SUBST(ACLOCAL_AMFLAGS, "-I m4 -I .")

NAUTILUS_REQUIRED=2.32

AC_PROG_CC
AC_DISABLE_STATIC
AC_PROG_LIBTOOL

dnl Check for pkgconfig first
AC_CHECK_PROG(HAVE_PKGCONFIG, pkg-config, yes, no)
AC_PATH_PROG(PKG_CONFIG, pkg-config, no)

dnl Check for gtk-doc
GTK_DOC_CHECK(1.14)

dnl Give error and exit if we don't have pkgconfig
if test "x$HAVE_PKGCONFIG" = "xno"; then
  AC_MSG_ERROR(you need to have pkgconfig installed !)
fi

AC_PREFIX_DEFAULT([$(pkg-config --variable=prefix libnautilus-extension || echo /usr)])

dnl **************************************************
dnl * Check for Python
dnl **************************************************
PYG_CHECK_PYTHON_HEADERS(,[AC_MSG_ERROR(could not find Python headers)])
PYG_CHECK_PYTHON_LIBS(,[AC_MSG_ERROR(could not find Python lib)])

if test "`pkg-config --variable=datadir pygobject-3.0`" != "" ; then
    PYGOBJECT_VERSION=pygobject-3.0
    PYGOBJECT_MAJOR_VERSION=3
    PYGOBJECT_MINOR_VERSION=0
    PYGOBJECT_MICRO_VERSION=0
else
    PYGOBJECT_VERSION=pygobject-2.0
    PYGOBJECT_MAJOR_VERSION=2
    PYGOBJECT_MINOR_VERSION=28
    PYGOBJECT_MICRO_VERSION=2
fi

PYGOBJECT_REQUIRED=$PYGOBJECT_MAJOR_VERSION.$PYGOBJECT_MINOR_VERSION.$PYGOBJECT_MICRO_VERSION
AC_DEFINE_UNQUOTED(PYGOBJECT_MAJOR_VERSION,[$PYGOBJECT_MAJOR_VERSION], [PyGObject major version.])
AC_DEFINE_UNQUOTED(PYGOBJECT_MINOR_VERSION,[$PYGOBJECT_MINOR_VERSION], [PyGObject minor version.])
AC_DEFINE_UNQUOTED(PYGOBJECT_MICRO_VERSION,[$PYGOBJECT_MICRO_VERSION], [PyGObject micro version.])

PKG_CHECK_MODULES(NAUTILUS_PYTHON, [$PYGOBJECT_VERSION >= $PYGOBJECT_REQUIRED
                                   libnautilus-extension >= $NAUTILUS_REQUIRED])

PYGOBJECT_DATADIR=`$PKG_CONFIG --variable=datadir $PYGOBJECT_VERSION`
AC_SUBST(PYGOBJECT_DATADIR)

NAUTILUS_LIBDIR=`$PKG_CONFIG --variable=libdir libnautilus-extension`
AC_SUBST(NAUTILUS_LIBDIR)
AC_DEFINE_UNQUOTED(NAUTILUS_LIBDIR, "$NAUTILUS_LIBDIR", [Nautilus libdir])

dnl ---------------------------------
dnl Get nautilus extensions directory
dnl ---------------------------------
NAUTILUS_EXTENSION_DIR=`$PKG_CONFIG --variable=extensiondir libnautilus-extension`
AC_SUBST(NAUTILUS_EXTENSION_DIR)
AC_DEFINE_UNQUOTED(NAUTILUS_EXTENSION_DIR, "$NAUTILUS_EXTENSION_DIR", [Nautilus' extensions directory])

dnl ---------------------------------
dnl Check for gtk-doc dependencies
dnl ---------------------------------
if test "${enable_gtk_doc}" != no; then
    dnl Check for gtkdoc-mkhtml
    AC_PATH_PROG([GTKDOC_MKHTML], [gtkdoc-mkhtml])
    if test -z "$GTKDOC_MKHTML"; then
        echo " disabling generation of docs"
        enable_gtk_doc=no
    fi
fi

AC_OUTPUT([
  Makefile
  examples/Makefile
  src/Makefile
  nautilus-python.pc
  docs/Makefile
  docs/reference/entities.docbook
])

echo
echo " nautilus-python $VERSION"
echo
echo "    Nautilus Prefix: ${prefix}"
echo "  PyGObject Version: ${PYGOBJECT_VERSION}"
echo "     Python Library: ${PYTHON_LIB_LOC}/${PYTHON_LIB_NAME}"
echo "      Documentation: ${enable_gtk_doc}"
echo
