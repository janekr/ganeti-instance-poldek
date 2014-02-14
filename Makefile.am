osname=$(subst ganeti-instance-,,$(PACKAGE))

osdir=$(OS_DIR)/$(osname)
variantsdir=$(sysconfdir)/ganeti/instance-poldek/variants
defaultsdir=$(sysconfdir)/default

dist_os_SCRIPTS = create import export rename verify
dist_os_DATA = ganeti_api_version variants.list parameters.list \
	       packages.list packages.systemd.list packages.sysvinit.list
os_DATA = common.sh

dist_doc_DATA = COPYING NEWS README

exampledir = $(docdir)/examples
exampleconfdir = $(exampledir)/confdata

dist_exampleconf_DATA = \
	examples/hooks/confdata/defaultpasswords \
	examples/hooks/confdata/timezone

dist_example_DATA = \
	examples/hooks/defaultpasswords \
	examples/hooks/grub \
	examples/hooks/hosts \
	examples/hooks/interfaces \
	examples/hooks/ssh-keys \
	examples/hooks/timezone

dist_variants_DATA = \
	variants/th.conf \
	variants/th-i686.conf

EXTRA_DIST = \
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

variants/th.conf: variants/th.conf.in
	$(do_subst) < $< > $@

variants/th-i686.conf: variants/th-i686.conf.in
	$(do_subst) < $< > $@

install-exec-local:
	@mkdir_p@ $(DESTDIR)$(variantsdir)
	touch $(DESTDIR)$(variantsdir)/default.conf
	@mkdir_p@ $(DESTDIR)$(defaultsdir)
	$(INSTALL_DATA) $(top_srcdir)/defaults \
		$(DESTDIR)$(defaultsdir)/ganeti-instance-poldek

uninstall-local:
	rm -f $(DESTDIR)$(variantsdir)/default.conf
	rm -f $(DESTDIR)$(variantsdir)/th.conf
	rm -f $(DESTDIR)$(variantsdir)/th-i686.conf
	rm -f $(DESTDIR)$(defaultsdir)/ganeti-instance-poldek

CLEANFILES = common.sh
