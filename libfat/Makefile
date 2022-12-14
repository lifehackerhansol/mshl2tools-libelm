ifeq ($(strip $(DEVKITPRO)),)
$(error "Please set DEVKITPRO in your environment. export DEVKITPRO=<path to>devkitPro)
endif
 
export TOPDIR	:=	$(CURDIR)
 
export LIBFAT_MAJOR	:= 1
export LIBFAT_MINOR	:= 0
export LIBFAT_PATCH	:= 10

export VERSTRING	:=	$(LIBFAT_MAJOR).$(LIBFAT_MINOR).$(LIBFAT_PATCH)


default: release

all: release dist

release: include/libfatversion.h nds-release gba-release cube-release wii-release

ogc-release: cube-release wii-release

nds-release:
	$(MAKE) -C nds BUILD=release

gba-release:
	$(MAKE) -C gba BUILD=release

cube-release:
	$(MAKE) -C libogc PLATFORM=cube BUILD=cube_release

wii-release:
	$(MAKE) -C libogc PLATFORM=wii BUILD=wii_release

debug: nds-debug gba-debug cube-debug wii-debug

ogc-debug: cube-debug wii-debug

nds-debug:
	$(MAKE) -C nds BUILD=debug

gba-debug:
	$(MAKE) -C gba BUILD=debug


cube-debug:
	$(MAKE) -C libogc PLATFORM=cube BUILD=wii_debug

wii-debug:
	$(MAKE) -C libogc PLATFORM=wii BUILD=cube_debug

clean: nds-clean gba-clean ogc-clean

nds-clean:
	$(MAKE) -C nds clean

gba-clean:
	$(MAKE) -C gba clean

ogc-clean:
	$(MAKE) -C libogc clean

dist-bin: nds-dist-bin gba-dist-bin ogc-dist-bin

nds-dist-bin: nds-release distribute/$(VERSTRING)
	$(MAKE) -C nds dist-bin

gba-dist-bin: gba-release distribute/$(VERSTRING)
	$(MAKE) -C gba dist-bin

ogc-dist-bin: ogc-release distribute/$(VERSTRING)
	$(MAKE) -C libogc dist-bin

dist-src: distribute/$(VERSTRING)
	@tar --exclude=.svn --exclude=*CVS* -cvjf distribute/$(VERSTRING)/libfat-src-$(VERSTRING).tar.bz2 \
	source include Makefile \
	nds/Makefile nds/include \
	gba/Makefile gba/include \
	libogc/Makefile libogc/include

dist: include/libfatversion.h dist-bin dist-src

distribute/$(VERSTRING):
	@[ -d $@ ] || mkdir -p $@

include/libfatversion.h : Makefile
	@echo "#ifndef __LIBFATVERSION_H__" > $@
	@echo "#define __LIBFATVERSION_H__" >> $@
	@echo >> $@
	@echo "#define _LIBFAT_MAJOR_	$(LIBFAT_MAJOR)" >> $@
	@echo "#define _LIBFAT_MINOR_	$(LIBFAT_MINOR)" >> $@
	@echo "#define _LIBFAT_PATCH_	$(LIBFAT_PATCH)" >> $@
	@echo >> $@
	@echo '#define _LIBFAT_STRING "libFAT Release '$(LIBFAT_MAJOR).$(LIBFAT_MINOR).$(LIBFAT_PATCH)'"' >> $@
	@echo >> $@
	@echo "#endif // __LIBFATVERSION_H__" >> $@

install: nds-install gba-install ogc-install

nds-install: nds-release
	$(MAKE) -C nds install

gba-install: gba-release
	$(MAKE) -C gba install

ogc-install: cube-release wii-release
	$(MAKE) -C libogc install
