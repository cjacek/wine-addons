AC_PREREQ([2.69])
AC_INIT([wine-distro],[0.1])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_SRCDIR(Makefile.in)

AC_CANONICAL_HOST
AC_SUBST(CROSSTARGET)
AC_SUBST(CROSSCPU)

AC_ARG_WITH([wine-tools],  AS_HELP_STRING([--with-wine-tools=PATH],  [use Wine tools from directory DIR]))
AC_ARG_WITH([wine-objdir], AS_HELP_STRING([--with-wine-objdir=PATH], [use Wine libs from directory DIR]))
AC_ARG_WITH([wine-srcdir], AS_HELP_STRING([--with-wine-srcdir=PATH], [use Wine headers from directory DIR]))

AC_ARG_ENABLE(win64, AS_HELP_STRING([--enable-win64],[build win64 version]))

if test -z "$CROSSTARGET"
then
  case $host in
    x86_64*|amd64*|i[[3456789]]86*)
      if test "x$enable_win64" != "xyes"
      then
        CROSSTARGET=i686-w64-mingw32
      else
        CROSSTARGET=x86_64-w64-mingw32
      fi ;;
    *)
      CROSSTARGET=$host_cpu-w64-mingw32 ;;
  esac
fi
save_IFS=$IFS
IFS=-
set x $CROSSTARGET
shift
CROSSCPU=$1
IFS=$save_IFS

AC_SUBST(WINEGCC)
AC_SUBST(WINEGCC_FLAGS,"-target $CROSSTARGET -mno-cygwin --lib-suffix=.cross.a")
AC_SUBST(CROSSCFLAGS)
if test -z "$CROSSCFLAGS"
then
  # workaround for Wine headers causing a ton of warnings
  CROSSCFLAGS="-Wno-pragma-pack"
fi

if test -z "$WINEGCC"
then
  if test -n "$with_wine_tools"
  then
    WINEGCC="$with_wine_tools/tools/winegcc/winegcc"
    WINEGCC_FLAGS="$WINEGCC_FLAGS --winebuild $with_wine_tools/tools/winebuild/winebuild"
  else
    WINEGCC=winegcc
  fi
fi

with_wine_objdir=${with_wine_objdir:-$with_wine_tools}
if test -n "$with_wine_objdir"
then
  WINEGCC_FLAGS="$WINEGCC_FLAGS --wine-objdir $with_wine_objdir"
  CROSSCFLAGS="$CROSSCFLAGS -I$with_wine_objdir/include"
fi

if test -n "$with_wine_srcdir"
then
  CROSSCFLAGS="$CROSSCFLAGS -I$with_wine_srcdir/include -I$with_wine_srcdir/include/msvcrt"
fi

AC_ARG_WITH([sdl2-srcdir],  AS_HELP_STRING([--with-sdl2-srcdir=PATH],  [use SDL2 sources tools from directory DIR]),
            [], [with_sdl2_srcdir="$srcdir/SDL"])
AC_ARG_WITH([faudio-srcdir],  AS_HELP_STRING([--with-faudio-srcdir=PATH],  [use FAudio sources tools from directory DIR]),
            [], [with_faudio_srcdir="$srcdir/FAudio"])
AC_ARG_WITH([freetype2-srcdir],  AS_HELP_STRING([--with-freetype2-srcdir=PATH],  [use freetype2 sources tools from directory DIR]),
            [], [with_freetype2_srcdir="$srcdir/freetype2"])

test "x$with_sdl2_srcdir" = "xno" || ln -sf "$with_sdl2_srcdir" sdl2-src
test "x$with_faudio_srcdir" = "xno" || ln -sf "$with_faudio_srcdir" faudio-src
test "x$with_freetype2_srcdir" = "xno" || ln -sf "$with_freetype2_srcdir" freetype2-src

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
