osname=$(subst ganeti-instance-,,$(PACKAGE))

osdir=$(OS_DIR)/$(osname)
hooksdir=$(sysconfdir)/ganeti/instance-poldek/hooks
variantsdir=$(sysconfdir)/ganeti/instance-poldek/variants
defaultsdir=$(sysconfdir)/default

dist_os_SCRIPTS = create import export rename verify
dist_os_DATA = ganeti_api_version variants.list parameters.list \
	       packages.list packages.systemd.list packages.sysvinit.list
os_DATA = common.sh

dist_doc_DATA = COPYING README

exampledir = $(docdir)/examples
exampleconfdir = $(exampledir)/confdata

dist_exampleconf_DATA = \
	examples/hooks/confdata/defaultpasswords \
	examples/hooks/confdata/timezone

dist_example_DATA = \
	examples/hooks/defaultpasswords \
	examples/hooks/grub2 \
	examples/hooks/hosts \
	examples/hooks/ssh-keys \
	examples/hooks/timezone

variants_DATA = \
	variants/th-2013.conf \
	variants/th-2013-i686.conf \
	variants/th.conf \
	variants/th-i686.conf

EXTRA_DIST = \
	variants/th-2013.conf.in \
	variants/th-2013-i686.conf.in \
	variants/th.conf.in \
	variants/th-i686.conf.in \
	common.sh.in \
	defaults

do_subst = sed \
	-e 's,[@]sysconfdir[@],$(sysconfdir),g' \
	-e 's,[@]localstatedir[@],$(localstatedir),g'

common.sh: common.sh.in Makefile
	$(do_subst) < $< > $@
	chmod +x $@

variants/%.conf: variants/%.conf.in
	$(do_subst) < $< > $@

install-data-hook:
	@mkdir_p@ $(DESTDIR)$(variantsdir)
	@mkdir_p@ $(DESTDIR)$(hooksdir)
	touch $(DESTDIR)$(variantsdir)/default.conf
	@if [ `uname -m` != "x86_64" ]; then\
		rm -f $(DESTDIR)$(variantsdir)/th-i686.conf;\
		rm -f $(DESTDIR)$(variantsdir)/th-2013-i686.conf;\
	else\
		echo "th-i686" >>$(DESTDIR)$(osdir)/variants.list;\
		echo "th-2013-i686" >>$(DESTDIR)$(osdir)/variants.list;\
	fi
	@mkdir_p@ $(DESTDIR)$(defaultsdir)
	$(INSTALL_DATA) $(top_srcdir)/defaults \
		$(DESTDIR)$(defaultsdir)/ganeti-instance-poldek

uninstall-hook:
	rm -f $(DESTDIR)$(variantsdir)/default.conf
	rm -f $(DESTDIR)$(variantsdir)/th.conf
	rm -f $(DESTDIR)$(variantsdir)/th-i686.conf
	rm -f $(DESTDIR)$(variantsdir)/th-2013.conf
	rm -f $(DESTDIR)$(variantsdir)/th-2013-i686.conf
	rm -f $(DESTDIR)$(defaultsdir)/ganeti-instance-poldek

CLEANFILES = common.sh
