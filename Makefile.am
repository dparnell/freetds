# version $Id: Makefile.am,v 1.43 2006/03/24 21:59:51 jklowden Exp $

## SUBDIRS determines in which directories automake will generate a Makefile
## See also AC_OUTPUT in configure.in

SUBDIRS	=	include src doc samples win32

ACLOCAL_AMFLAGS = -I m4

VMS	=	vms/config_h.vms \
		vms/configure.com \
		vms/descrip_mms.template \
		vms/getpass.c \
		vms/libodbc.opt \
		vms/odbc_driver_axp.opt \
		vms/README.vms

EXTRA_DIST = 	$(VMS) interfaces PWD.in BUGS \
		freetds.conf locales.conf \
		autogen.sh freetds.spec.in freetds.spec tds.dox 


ETC	=	$(DESTDIR)$(sysconfdir)
DOCDIR	=	doc/doc/freetds-$(VERSION)

install-data-local:
	$(mkinstalldirs) $(ETC)
	if test ! -f $(ETC)/freetds.conf; then \
		$(INSTALL_DATA) $(srcdir)/freetds.conf $(ETC)/freetds.conf; \
	fi
	if test ! -f $(ETC)/locales.conf; then \
		$(INSTALL_DATA) $(srcdir)/locales.conf $(ETC)/locales.conf; \
	fi

clean-local:
	find . \( -name \*.test_output -o -name \*.bb -o -name \*.bbg -o -name \*.da -o -name \*.gc\* \) -exec rm -f {} \;

# enable some cheat so make distcheck work (see below)
DISTCHECK_CONFIGURE_FLAGS	=	\
	--enable-distcheck-build --enable-extra-checks

# do not uninstall configuration files (they may have been changed)
if DISTCHECK_BUILD
uninstall-local:
	rm -f $(ETC)/freetds.conf $(ETC)/locales.conf $(ETC)/pool.conf PWD
endif

test:
	@echo "The 'make test' option has been replaced with 'make check'";

all:
if DISTCHECK_BUILD
# if we are inside a make distcheck copy our real password file
	if test ! -f PWD -a -f $(srcdir)/../PWD; then cp $(srcdir)/../PWD PWD; fi
endif
# this prevent the store of passwords in CVS
	if test ! -f PWD; then cp $(srcdir)/PWD.in PWD; fi

snapshot:
	$(MAKE) dist distdir=$(PACKAGE)-`date +"%Y%m%d"`

## The doxygen configuration file understands environment variables.  
doxy: $(SUBDIRS)
if HAVE_DOXYGEN
	$(mkinstalldirs) $(DOCDIR)/reference
	rm -rf $(DOCDIR)/reference/*
	SRCDIR=$(top_srcdir) VERSION=$(VERSION) $(DOXYGEN) $(top_srcdir)/tds.dox
	if test -d $(DOCDIR)/reference/html ; then mv $(DOCDIR)/reference/html/* $(DOCDIR)/reference; rmdir $(DOCDIR)/reference/html; fi
else
	echo 'Doxygen not installed; reference manual not built.'
endif

