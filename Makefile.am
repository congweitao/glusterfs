SOURCES = site.h

EXTRA_DIST = autogen.sh \
	COPYING-GPLV2 COPYING-LGPLV3 COMMITMENT \
	INSTALL README.md AUTHORS THANKS NEWS \
	glusterfs.spec glusterfs-api.pc.in libgfchangelog.pc.in libgfdb.pc.in \
	run-tests.sh \
	build-aux/pkg-version \
	contrib/umountd \
	$(shell find $(top_srcdir)/tests -type f -print)

SUBDIRS = $(ARGP_STANDALONE_DIR) rpc/xdr/gen libglusterfs rpc api xlators \
	glusterfsd $(FUSERMOUNT_SUBDIR) doc extras cli heal \
	@SYNCDAEMON_SUBDIR@ @UMOUNTD_SUBDIR@ tools events

pkgconfigdir = @pkgconfigdir@
pkgconfig_DATA = glusterfs-api.pc libgfchangelog.pc
if USE_GFDB
pkgconfig_DATA += libgfdb.pc
endif

CLEANFILES = glusterfs-api.pc libgfchangelog.pc libgfdb.pc \
	contrib/umountd/Makefile

gitclean: distclean
	find . -name Makefile.in -exec rm -f {} \;
	find . -name mount.glusterfs -exec rm -f {} \;
	rm -fr autom4te.cache
	rm -f missing aclocal.m4 config.h.in config.guess config.sub ltmain.sh install-sh configure depcomp

# dist-hook gets executed with 'make dist', this is the only target getting
# executed, a dist-hook in other Makefile.am files seem to get ignored.
dist-hook: gen-VERSION gen-ChangeLog
	-rm -fr $(distdir)/contrib/umountd/.deps
	-rm -f $(distdir)/events/src/eventtypes.py
	-rm -f $(distdir)/tests/env.rc
	-cp -f $(top_srcdir)/build-aux/config.sub.dist $(distdir)/config.sub
	-cp -f $(top_srcdir)/build-aux/config.guess.dist $(distdir)/config.guess

.PHONY: gen-VERSION gen-ChangeLog clang-check

clang-check:
	@$(top_srcdir)/extras/clang-checker.sh

gen-ChangeLog:
	(cd $(srcdir) && git diff && echo ===== git log ==== && git log) > $(distdir)/ChangeLog

.PHONY : gen-VERSION
gen-VERSION:
	if test -d $(top_srcdir)/.git; then             \
	  cd $(top_srcdir);                             \
	  ./build-aux/pkg-version --full                \
	    > $(abs_top_builddir)/$(distdir)/VERSION;   \
	fi

