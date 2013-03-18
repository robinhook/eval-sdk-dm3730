#$L$
# Copyright (C) 2010 Ridgerun (http://www.ridgerun.com). 
#$L$

include ../../../bsp/classes/rrsdk.defs
include $(CLASSES)/fetcher.defs
include ../mach/Makefile.rules

XLOADER_TARBALL?=$(XLOADER).tar.gz
PKG_TARBALL=$(XLOADER_TARBALL)
FETCHER_EXTRACT_DIRECTORY_NAME=src
PKG_URL=$(XLOADER_URL)

.PHONY: build clean

all: xloader_patched xloader_configured xloader_built images

ifneq (x$(XLOADER_TARBALL),x)
XLOADER_URL?=https://$(DOWNLOAD_SERVER)/packages/
endif

build: xloader_patched xloader_built images

xloader_built: $(XLOADER) xloader_patched xloader_configured
	$(V) $(ECHO) "Building $(XLOADER)-$(XLOADER_VERSION)..."
	$(V) $(MAKE) -C src all CROSS_COMPILE=$(TOOLCHAIN_PREFIX)- CFG_LOADADDRS=$(CONFIG_UBOOT_RAM_ADDRS) $(QOUT) $(ERRQOUT)
	$(V) touch $@

xloader_configured: xloader_patched
	$(V) echo ;
	$(V) echo "Configuring for $(XLOADER_BOARD_CONFIG) ..."
	$(V) $(MAKE) -C src $(XLOADER_BOARD_CONFIG) CROSS_COMPILE=$(TOOLCHAIN_PREFIX)- $(QOUT)
	$(V) touch $@

xloader_patched: $(XLOADER) rrpatched
	touch $@

ifneq (x$(XLOADER_URL),x)
$(XLOADER): rrfetched
else
$(XLOADER):
	$(V)$(ECHO) "\n$${ERROR_COLOR}Error:$${NORMAL_COLOR} no XLOADER_URL or XLOADER_TARBALL defined, and xloader code not found... aborting\n";
	$(V) exit -1;
endif

images:
	$(V) cp src/x-load.bin.ift $(DEVDIR)/images/

clean: unpatch
	$(V) if [ -f rrfetched ] ; then \
	    rm -Rf src rrfetched ; \
	else \
	    if [ -d src ] ; then \
		$(MAKE) -C src CROSS_COMPILE=$(TOOLCHAIN_PREFIX)- distclean \
		    $(QOUT) 2>$(LOGFILE) ; \
	    fi ; \
	fi
	$(V) rm -f src/x-load.bin.ift src/signGP patched
	$(V) rm -f *.o *.d xloader_configured xloader_patched xloader_built
