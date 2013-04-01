#$L$
# Copyright (C) 2011 Ridgerun (http://www.ridgerun.com).
##$L$

PKG_URL=git://git.ideasonboard.org/media-ctl.git
PKG_TYPE=git
PKG_REVISION=b16cbe7bce0fd8d9d940473a2272b6b0148795c3

AUTOTOOLS_AUTOGEN_GENERATION_RULE = src/autogen.sh

# Custom Autotools parameters
APPS_LDFLAGS+=-Wl,--rpath-link -Wl,$(FSDEVROOT)/usr/lib
CFLAGS+=-I$(DEVDIR)/kernel/$(KERNEL)/include -I$(FSDEVROOT)/usr/include

AUTOTOOLS_PARAMS += --host="$(ARCH)-linux" CFLAGS="$(CFLAGS)" CPPFLAGS="$(CPPFLAGS)" LDFLAGS="$(APPS_LDFLAGS)"


BINARIES=/usr/bin/media-ctl
LIBRARIES=/usr/lib/{libmediactl.so.0.0.0,libv4l2subdev.so.0.0.0}



include ../../../bsp/classes/rrsdk.class
include $(CLASSES)/autotools.class

src/autogen.sh: src/debian/rules
	$(V) ln -sf debian/rules src/autogen.sh;
