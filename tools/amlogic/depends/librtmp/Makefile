include ../../Makefile.include
include ../depends.mk

# lib name, version
LIBNAME=rtmpdump
VERSION=2.4
SOURCE=$(LIBNAME)-$(VERSION)
ARCHIVE=$(SOURCE).tar.gz

# configuration settings

LIBDYLIB=$(SOURCE)/$(LIBNAME)/.libs/$(LIBNAME).so

CLEAN_FILES=$(ARCHIVE) $(SOURCE)

all: $(LIBDYLIB) .installed

$(TARBALLS_LOCATION)/$(ARCHIVE):
	$(RETRIEVE_TOOL) $(RETRIEVE_TOOL_FLAGS) $(BASE_URL)/$(ARCHIVE)

$(SOURCE): $(TARBALLS_LOCATION)/$(ARCHIVE)
	rm -rf $(SOURCE)
	$(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	echo $(SOURCE) > .gitignore
	cd $(SOURCE)/librtmp; patch -p0 < ../../prefix.patch

$(LIBDYLIB): $(SOURCE)
	make -j $(JOBS) -C $(SOURCE)/librtmp CROSS_COMPILE=$(TOOLCHAIN)/bin/$(HOST)- PREFIX=$(PREFIX) CC="$(CC)" LD="$(LD)"

.installed:
	make -C $(SOURCE)/librtmp install
	touch $@

clean:
	make -C $(SOURCE) clean
	rm -f .installed

distclean::
	rm -rf $(SOURCE) .installed

