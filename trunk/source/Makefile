
ifeq ($(ARCH),)
REALARCH=$(shell uname -m)
ifeq ($(REALARCH),x86_64)
ARCH=i386
else
ARCH=$(REALARCH)
endif
endif
ifeq ($(shell id -u),0)
$(error Sorry, this script cannot be directly run as root.  Run "make fulldepends" instead)
endif

ifneq ($(ARCH),x86_64)
INSTALLFLAGS+=--force-32bit
CHROMIUM_ROOT_DEPS=chromium-root
AWESOMIUM_DEPS=installed-awesomium
endif

# Default rule:
depends: check-architecture optionaldepends

minimaldepends: check-architecture nonroot-dependencies compiled-dependencies

optionaldepends: check-architecture nonroot-dependencies optional-dependencies compiled-dependencies

monooisdepends: check-architecture optionaldepends installed-mono installed-ois

fulldepends: check-architecture root-dependencies compiled-dependencies optional-dependencies

awesomium-root: chromium-root
	$(MAKE) installed-awesomium $(*)

chromium-root:
	sudo $(filter -j%,$(MFLAGS)) ./install $(INSTALLFLAGS) --use-root --user `whoami` chromiumdeps
	$(MAKE) installed-chromium $(*)

check-architecture:
	@if [ x"$(ARCH)" = x"x86_64" ]; then \
	    if [ -e arch-32bit ]; then \
                echo This dependencies folder was compiled for ARCH=i386, but you specified ARCH=$(ARCH). ; \
	        false; \
	    else \
	        touch arch-64bit; \
	    fi \
	else \
	    if [ -e arch-64bit ]; then \
                echo This dependencies folder was compiled for ARCH=x86_64, but you specified ARCH=$(ARCH). ; \
	        false; \
	    else \
	        touch arch-32bit; \
	    fi \
	fi

##### Rules to install each package
installed-%:
	$(MAKE) check-architecture $(MFLAGS)
	$(filter -j%,$(MFLAGS)) ./install $(INSTALLFLAGS) $(patsubst installed-%,%,$@)

installed-ogre: installed-freeimage installed-boost installed-sdl

installed-awesomium: installed-chromium

#####

compiled-dependencies: installed-antlr installed-protobufs installed-sqlite

optional-dependencies: installed-ogre installed-bullet installed-mono $(AWESOMIUM_DEPS)

nonroot-dependencies: installed-curl installed-boost

root-dependencies: nonroot-dependencies $(CHROMIUM_ROOT_DEPS)
	sudo ./install $(INSTALLFLAGS) --use-root --user `whoami`

clean:
	@echo "Clean command:"
	@echo rm -rf `svn st | grep '^?' | grep -v 'protobuf-2.1.0.tar.gz' | cut -c 3-`
	@echo "Are you sure you want to clean the entire dependencies directory?"
	@read CONFIRM; if ! [ x"${CONFIRM#y}" = x"${CONFIRM}" ]; then rm -rf `svn st | grep '^?' | grep -v 'protobuf-2.1.0.tar.gz' | cut -c 3-`; fi
