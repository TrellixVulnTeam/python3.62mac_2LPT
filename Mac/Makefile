# This file can be invoked from the various frameworkinstall... targets in the
# main Makefile. The next couple of variables are overridden on the
# commandline in that case.

VERSION=3.6
ABIFLAGS=dm
LDVERSION=$(VERSION)$(ABIFLAGS)
ENSUREPIP=upgrade
builddir = ..
srcdir=.
prefix=/usr/local/Cellar/python3/3.6.2_1/Frameworks/Python.framework/Versions/3.6
exec_prefix=${prefix}
LIBDEST=$(prefix)/lib/python$(VERSION)
RUNSHARED=DYLD_FRAMEWORK_PATH=/Users/dojo/work/zspecial/python3.62mac
BUILDEXE=.exe
BUILDPYTHON=$(builddir)/python$(BUILDEXE)
DESTDIR=
LDFLAGS=-L/usr/local/opt/lapack/lib -L/usr/local/opt/openblas/lib -L/usr/local/opt/llvm/lib -L/usr/local/opt/sqlite/lib -L/usr/local/opt/readline/lib -L/usr/local/opt/openssl@1.1/lib 
FRAMEWORKUNIXTOOLSPREFIX=/usr/local/Cellar/python3/3.6.2_1
PYTHONFRAMEWORK=Python
PYTHONFRAMEWORKIDENTIFIER=org.python.python
LIPO_32BIT_FLAGS=
CC=mpicc
MACOSX_DEPLOYMENT_TARGET=10.13
export MACOSX_DEPLOYMENT_TARGET



# These are normally glimpsed from the previous set
BINDIR=		${exec_prefix}/bin
PYTHONAPPSDIR=/Applications/$(PYTHONFRAMEWORK) $(VERSION)
APPINSTALLDIR=$(prefix)/Resources/Python.app

# Variables for installing the "normal" unix binaries
INSTALLED_PYTHONAPP=$(APPINSTALLDIR)/Contents/MacOS/$(PYTHONFRAMEWORK)

# Items more-or-less copied from the main Makefile
DIRMODE=755
FILEMODE=644
INSTALL=/usr/local/opt/coreutils/libexec/gnubin/install -c
INSTALL_SYMLINK=ln -fsn
INSTALL_PROGRAM=${INSTALL}
INSTALL_SCRIPT= ${INSTALL}
INSTALL_DATA=${INSTALL} -m 644
LN=ln
STRIPFLAG=-s
CPMAC=CpMac

APPTEMPLATE=$(srcdir)/Resources/app
APPSUBDIRS=MacOS Resources
compileall=$(srcdir)/../Lib/compileall.py

installapps: install_Python install_PythonLauncher install_IDLE

#
# Install unix tools in /usr/local/bin. These are just aliases for the
# actual installation inside the framework.
#
installunixtools:
	if [ ! -d "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" ]; then  \
		$(INSTALL) -d -m $(DIRMODE) "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" ;\
	fi
	cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
	for fn in \
			2to3 \
			idle3 \
			pydoc3 \
			python3 \
			python3-config \
			pyvenv \
			; \
	do \
		rm -f $${fn} ; \
		$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
	done
	-if test "x$(VERSION)" != "x$(LDVERSION)"; then \
		cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
		for fn in \
				python$(VERSION)-config \
				; \
		do \
			rm -f $${fn} ;\
			$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
		done ;\
	fi
	-if test "x$(LIPO_32BIT_FLAGS)" != "x"; then \
		cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
		for fn in \
				python3-32 \
				; \
		do \
			rm -f $${fn} ;\
			$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
		done ;\
	fi
	-if test "x$(ENSUREPIP)" != "xno"  ; then \
		cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
		for fn in \
				pip3 \
				; \
		do \
			rm -f $${fn} ;\
			$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
		done ;\
	fi

#
# Like installunixtools, but only install links to the versioned binaries.
#
altinstallunixtools:
	if [ ! -d "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" ]; then  \
		$(INSTALL) -d -m $(DIRMODE) "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" ;\
	fi
	cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
	for fn in \
			2to3-$(VERSION) \
			idle$(VERSION) \
			pydoc$(VERSION) \
			python$(VERSION) \
			python$(LDVERSION)-config \
			pyvenv-$(VERSION) \
			; \
	do \
		rm -f $${fn} ;\
		$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
	done
	-if test "x$(VERSION)" != "x$(LDVERSION)"; then \
		cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
		for fn in \
				python$(LDVERSION) \
				; \
		do \
			rm -f $${fn} ;\
			$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
		done ;\
	fi
	-if test "x$(LIPO_32BIT_FLAGS)" != "x"; then \
		cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
		for fn in \
				python$(VERSION)-32 \
				; \
		do \
			rm -f $${fn} ;\
			$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
		done ;\
	fi
	-if test "x$(ENSUREPIP)" != "xno"  ; then \
		cd "$(DESTDIR)$(FRAMEWORKUNIXTOOLSPREFIX)/bin" && \
		for fn in \
				easy_install-$(VERSION) \
				pip$(VERSION) \
				; \
		do \
			rm -f $${fn} ;\
			$(LN) -s $(BINDIR)/$${fn} $${fn} ;\
		done ;\
	fi

pythonw: $(srcdir)/Tools/pythonw.c Makefile
	$(CC) $(LDFLAGS) -DPYTHONFRAMEWORK='"$(PYTHONFRAMEWORK)"' -o $@ \
		$(srcdir)/Tools/pythonw.c -I.. -I$(srcdir)/../Include \
		../$(PYTHONFRAMEWORK).framework/Versions/$(VERSION)/$(PYTHONFRAMEWORK)

install_PythonLauncher:
	cd PythonLauncher && make install DESTDIR=$(DESTDIR)

install_Python:
	@for i in "$(PYTHONAPPSDIR)" "$(APPINSTALLDIR)" "$(APPINSTALLDIR)/Contents"; do \
		if test ! -d "$(DESTDIR)$$i"; then \
			echo "Creating directory $(DESTDIR)$$i"; \
			$(INSTALL) -d -m $(DIRMODE) "$(DESTDIR)$$i"; \
		fi;\
	done
	@for i in $(APPSUBDIRS); do \
		if test ! -d "$(DESTDIR)$(APPINSTALLDIR)/Contents/$$i"; then \
			echo "Creating directory $(DESTDIR)$(APPINSTALLDIR)/Contents/$$i"; \
			$(INSTALL) -d -m $(DIRMODE) "$(DESTDIR)$(APPINSTALLDIR)/Contents/$$i"; \
		else	true; \
		fi; \
	done
	@for d in . $(APPSUBDIRS); \
	do \
		a=$(APPTEMPLATE)/$$d; \
		if test ! -d $$a; then continue; else true; fi; \
		b="$(DESTDIR)$(APPINSTALLDIR)/Contents/$$d"; \
		for i in $$a/*; \
		do \
			case $$i in \
			*CVS) ;; \
			*.svn) ;; \
			*.py[co]) ;; \
			*.orig) ;; \
			*~) ;; \
			*idx) \
				echo $(CPMAC) "$$i" $$b; \
				$(CPMAC) "$$i" "$$b"; \
				;; \
			*) \
				if test -d $$i; then continue; fi; \
				if test -x $$i; then \
				    echo $(INSTALL_SCRIPT) "$$i" "$$b"; \
				    $(INSTALL_SCRIPT) "$$i" "$$b"; \
				else \
				    echo $(INSTALL_DATA) "$$i" "$$b"; \
				    $(INSTALL_DATA) "$$i" "$$b"; \
				fi;; \
			esac; \
		done; \
	done
	$(INSTALL_PROGRAM) $(STRIPFLAG) $(BUILDPYTHON) "$(DESTDIR)$(APPINSTALLDIR)/Contents/MacOS/$(PYTHONFRAMEWORK)"
	sed -e "s!%bundleid%!$(PYTHONFRAMEWORKIDENTIFIER)!g" \
		-e "s!%version%!`$(RUNSHARED) $(BUILDPYTHON) \
		-c 'import platform; print(platform.python_version())'`!g" \
		< "$(DESTDIR)$(APPINSTALLDIR)/Contents/Info.plist.in" \
		> "$(DESTDIR)$(APPINSTALLDIR)/Contents/Info.plist"
	rm "$(DESTDIR)$(APPINSTALLDIR)/Contents/Info.plist.in"


install_IDLE:
	test -d "$(DESTDIR)$(PYTHONAPPSDIR)" || mkdir -p "$(DESTDIR)$(PYTHONAPPSDIR)"
	-test -d "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app" && rm -rf "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app"
	/bin/cp -PR "$(srcdir)/IDLE/IDLE.app" "$(DESTDIR)$(PYTHONAPPSDIR)"
	ln -sf "$(INSTALLED_PYTHONAPP)" "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app/Contents/MacOS/Python"
ifneq ($(LIPO_32BIT_FLAGS),)
	rm "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app/Contents/MacOS/Python"
	lipo $(LIPO_32BIT_FLAGS) -output "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app/Contents/MacOS/Python" "$(BUILDPYTHON)"
endif
	sed -e "s!%prefix%!$(prefix)!g" -e 's!%exe%!$(PYTHONFRAMEWORK)!g' < "$(srcdir)/IDLE/IDLE.app/Contents/MacOS/IDLE" > "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app/Contents/MacOS/IDLE"
	sed "s!%version%!`$(RUNSHARED) $(BUILDPYTHON) -c 'import platform; print(platform.python_version())'`!g" < "$(srcdir)/IDLE/IDLE.app/Contents/Info.plist" > "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app/Contents/Info.plist"
	if [ -f "$(DESTDIR)$(LIBDEST)/idlelib/config-main.def" ]; then \
		/bin/cp -p "$(DESTDIR)$(LIBDEST)/idlelib/config-main.def" \
			"$(DESTDIR)$(LIBDEST)/idlelib/config-main.def~" ; \
		sed -e 's!name= IDLE Classic Windows!name= IDLE Classic OSX!g' \
			< "$(DESTDIR)$(LIBDEST)/idlelib/config-main.def~" \
			> "$(DESTDIR)$(LIBDEST)/idlelib/config-main.def" ; \
		rm "$(DESTDIR)$(LIBDEST)/idlelib/config-main.def~" ; \
	fi
	if [ -f "$(DESTDIR)$(LIBDEST)/idlelib/config-extensions.def" ]; then \
		/bin/cp -p "$(DESTDIR)$(LIBDEST)/idlelib/config-extensions.def" \
			"$(DESTDIR)$(LIBDEST)/idlelib/config-extensions.def~" ; \
		sed -e 's!zoom-height=<Alt-Key-2>!zoom-height=<Option-Key-0>!g' \
			-e 's!<Alt-Key-!<Option-Key-!g' \
			< "$(DESTDIR)$(LIBDEST)/idlelib/config-extensions.def~" \
			> "$(DESTDIR)$(LIBDEST)/idlelib/config-extensions.def" ; \
		rm "$(DESTDIR)$(LIBDEST)/idlelib/config-extensions.def~" ; \
	fi
	touch "$(DESTDIR)$(PYTHONAPPSDIR)/IDLE.app"

$(INSTALLED_PYTHONAPP): install_Python

installextras: $(srcdir)/Extras.install.py
	$(INSTALL) -d "$(DESTDIR)$(prefix)/share/doc/python$(VERSION)/examples"
	$(RUNSHARED) $(BUILDPYTHON) $(srcdir)/Extras.install.py $(srcdir)/../Tools \
		"$(DESTDIR)$(prefix)/share/doc/python$(VERSION)/examples/Tools" ; \
		chmod -R ugo+rX,go-w "$(DESTDIR)$(prefix)/share/doc/python$(VERSION)/examples/Tools"

clean:
	rm pythonw
	cd PythonLauncher && make clean

Makefile: $(srcdir)/Makefile.in ../config.status
	cd .. && CONFIG_FILES=Mac/Makefile CONFIG_HEADERS= $(SHELL) ./config.status
