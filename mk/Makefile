ifdef B_BASE
include $(B_BASE)/common.mk
include $(B_BASE)/rpmbuild.mk
REPO = $(call gitloc,xen-api-libs)
else
REPO ?= $(CURDIR)
%/.dirstamp:
	@mkdir -p $*
	@touch $@
endif

RPM_BINDIR=$(RPM_RPMSDIR)/$(DOMAIN0_ARCH_OPTIMIZED)

.PHONY: build
build: srpm $(MY_SOURCES)/MANIFEST
	$(RPMBUILD) --target $(DOMAIN0_ARCH_OPTIMIZED) -bb $(RPM_SPECSDIR)/xapi-libs.spec
	mkdir -p $(MY_MAIN_PACKAGES)
	cp $(RPM_RPMSDIR)/$(DOMAIN0_ARCH_OPTIMIZED)/xapi-libs-fe-*.rpm $(MY_MAIN_PACKAGES)

.PHONY: srpm
srpm:
	mkdir -p $(RPM_SRPMSDIR) $(RPM_SPECSDIR) $(RPM_SOURCESDIR) $(RPM_RPMSDIR)
	while ! [ -d .git ]; do cd ..; done; \
	git archive --prefix=xapi-libs-0/ --format=tar HEAD | bzip2 -z > $(RPM_SOURCESDIR)/xapi-libs-0.tar.bz2  # xen-api-libs/mk/Makefile
	install ../xapi-libs.spec $(RPM_SPECSDIR)
	$(RPMBUILD) --target $(DOMAIN0_ARCH_OPTIMIZED) -bs $(RPM_SPECSDIR)/xapi-libs.spec

$(MY_SOURCES)/MANIFEST: $(MY_SOURCES_DIRSTAMP)
	rm -f $@
	/bin/sh ./srpms-to-manifest api-libs $(MY_OUTPUT_DIR)/SRPMS > $@

.PHONY: clean
	rm -f *.rpm
