AC_INIT
AM_INIT_AUTOMAKE(cdec,0.1)
AC_CONFIG_HEADERS(config.h)
AC_PROG_LIBTOOL
AC_PROG_LEX
case $LEX in 
:) AC_MSG_ERROR([No lex (Flex, lex, etc.) program found]);; 
esac 
AC_PROG_CC
AC_PROG_CXX
AC_LANG_CPLUSPLUS
BOOST_REQUIRE
BOOST_PROGRAM_OPTIONS
#BOOST_THREADS
CPPFLAGS="$CPPFLAGS $BOOST_CPPFLAGS"
LDFLAGS="$LDFLAGS $BOOST_PROGRAM_OPTIONS_LDFLAGS"
# $BOOST_THREAD_LDFLAGS"
LIBS="$LIBS $BOOST_PROGRAM_OPTIONS_LIBS"
# $BOOST_THREAD_LIBS"

AC_CHECK_HEADER(boost/math/special_functions/digamma.hpp,
               [AC_DEFINE([HAVE_BOOST_DIGAMMA], [], [flag for boost::math::digamma])])

AC_CHECK_HEADER(google/dense_hash_map,
               [AC_DEFINE([HAVE_SPARSEHASH], [], [flag for google::dense_hash_map])])

AC_PROG_INSTALL
GTEST_LIB_CHECK

AC_ARG_ENABLE(mpi,
 [ --enable-mpi  Build MPI binaries, assumes mpi.h is present ],
 [ mpi=yes
 ])
AM_CONDITIONAL([MPI], [test "x$mpi" = xyes])

if test "x$mpi" = xyes
then
  BOOST_SERIALIZATION
  AC_DEFINE([HAVE_MPI], [1], [flag for MPI])
  # TODO BOOST_MPI needs to be implemented
  LIBS="$LIBS -lboost_mpi $BOOST_SERIALIZATION_LIBS -lmpi++ -lmpi"
fi

AM_CONDITIONAL([RAND_LM], false)
AC_ARG_WITH(randlm,
            [AC_HELP_STRING([--with-randlm=PATH], [(optional) path to RandLM toolkit])],
            [with_randlm=$withval],
            [with_randlm=no]
           )

AM_CONDITIONAL([GLC], false)
AC_ARG_WITH(glc,
            [AC_HELP_STRING([--with-glc=PATH], [(optional) path to Global Lexical Coherence package (Context CRF)])],
            [with_glc=$withval],
            [with_glc=no]
           )
FF_GLC=""

if test "x$with_randlm" != 'xno'
then
  SAVE_CPPFLAGS="$CPPFLAGS"
  CPPFLAGS="$CPPFLAGS -I${with_randlm}/include"

  AC_CHECK_HEADER(RandLM.h,
                 [AC_DEFINE([HAVE_RANDLM], [], [flag for RandLM])],
                 [AC_MSG_ERROR([Cannot find RandLM!])])


  LIB_RANDLM="-lrandlm"
  LDFLAGS="$LDFLAGS -L${with_randlm}/lib"
  LIBS="$LIBS $LIB_RANDLM"
  FMTLIBS="$FMTLIBS librandlm.a"
  AM_CONDITIONAL([RAND_LM], true)
fi


if test "x$with_glc" != 'xno'
then
  SAVE_CPPFLAGS="$CPPFLAGS"
  CPPFLAGS="$CPPFLAGS -I${with_glc} -I${with_glc}/cdec"

  #AC_CHECK_HEADER(ff_glc.h,
  #               [AC_DEFINE([HAVE_GLC], [], [flag for GLC])],
  #               [AC_MSG_ERROR([Cannot find GLC!])])

  AC_DEFINE([HAVE_GLC], [], [flag for GLC])
  #LIB_RANDLM="-lrandlm"
  #LDFLAGS="$LDFLAGS -L${with_glc}/lib"
  #LIBS="$LIBS $LIB_RANDLM"
  #FMTLIBS="$FMTLIBS libglc.a"
  AC_SUBST(FF_GLC,"${with_glc}/cdec/ff_glc.cc")
  AM_CONDITIONAL([GLC], true)
fi

AC_OUTPUT(Makefile utils/Makefile mteval/Makefile extools/Makefile decoder/Makefile phrasinator/Makefile training/Makefile vest/Makefile klm/util/Makefile klm/lm/Makefile mira/Makefile gi/pyp-topics/src/Makefile gi/clda/src/Makefile)
