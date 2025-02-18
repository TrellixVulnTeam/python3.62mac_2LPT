CC=mpicc
LD=mpicc
BASECFLAGS= -Wsign-compare -fno-common -dynamic
OPT=-DDYNAMIC_ANNOTATIONS_ENABLED=1 -g -Ofast -Wall -Wstrict-prototypes
CFLAGS=-fomit-frame-pointer -funroll-loops  -ffast-math -Ofast -fno-signed-zeros -ffp-contract=fast -mmmx -msse -flto -maes -march=native  -march=native $(BASECFLAGS) $(OPT)
LDFLAGS=-L/usr/local/opt/lapack/lib -L/usr/local/opt/openblas/lib -L/usr/local/opt/llvm/lib -L/usr/local/opt/sqlite/lib -L/usr/local/opt/readline/lib -L/usr/local/opt/openssl@1.1/lib 
srcdir=         .
VERSION=	3.6
UNIVERSALSDK=
builddir=	../..

RUNSHARED=      DYLD_FRAMEWORK_PATH=/Users/dojo/work/zspecial/python3.62mac
BUILDEXE=       .exe
BUILDPYTHON=    $(builddir)/python$(BUILDEXE)
PYTHONFRAMEWORK=Python

# Deployment target selected during configure, to be checked
# by distutils
MACOSX_DEPLOYMENT_TARGET=10.13
export MACOSX_DEPLOYMENT_TARGET

PYTHONAPPSDIR=/Applications/$(PYTHONFRAMEWORK) $(VERSION)
OBJECTS=FileSettings.o MyAppDelegate.o MyDocument.o PreferencesWindowController.o doscript.o main.o

install: Python\ Launcher.app
	test -d "$(DESTDIR)$(PYTHONAPPSDIR)" || mkdir -p "$(DESTDIR)$(PYTHONAPPSDIR)"
	-test -d "$(DESTDIR)$(PYTHONAPPSDIR)/Python Launcher.app" && rm -r "$(DESTDIR)$(PYTHONAPPSDIR)/Python Launcher.app"
	/bin/cp -r "Python Launcher.app" "$(DESTDIR)$(PYTHONAPPSDIR)"
	touch "$(DESTDIR)$(PYTHONAPPSDIR)/Python Launcher.app"

clean:
	rm -f *.o "Python Launcher"
	rm -rf "Python Launcher.app"
	rm -f Info.plist

Python\ Launcher.app:  Info.plist \
		Python\ Launcher $(srcdir)/../Icons/PythonLauncher.icns \
		$(srcdir)/../Icons/PythonSource.icns \
		$(srcdir)/../Icons/PythonCompiled.icns \
		$(srcdir)/factorySettings.plist
	rm -fr "Python Launcher.app"
	mkdir "Python Launcher.app"
	mkdir "Python Launcher.app/Contents"
	mkdir "Python Launcher.app/Contents/MacOS"
	mkdir "Python Launcher.app/Contents/Resources"
	cp "Python Launcher" "Python Launcher.app/Contents/MacOS"
	cp Info.plist  "Python Launcher.app/Contents"
	cp $(srcdir)/../Icons/PythonLauncher.icns  "Python Launcher.app/Contents/Resources"
	cp $(srcdir)/../Icons/PythonSource.icns  "Python Launcher.app/Contents/Resources"
	cp $(srcdir)/../Icons/PythonCompiled.icns  "Python Launcher.app/Contents/Resources"
	cp $(srcdir)/factorySettings.plist  "Python Launcher.app/Contents/Resources"
	cp -R $(srcdir)/English.lproj "Python Launcher.app/Contents/Resources"

FileSettings.o: $(srcdir)/FileSettings.m
	$(CC) $(CFLAGS) -o $@ -c $(srcdir)/FileSettings.m

MyAppDelegate.o: $(srcdir)/MyAppDelegate.m
	$(CC) $(CFLAGS) -o $@ -c $(srcdir)/MyAppDelegate.m

MyDocument.o: $(srcdir)/MyDocument.m
	$(CC) $(CFLAGS) -o $@ -c $(srcdir)/MyDocument.m

PreferencesWindowController.o: $(srcdir)/PreferencesWindowController.m
	$(CC) $(CFLAGS) -o $@ -c $(srcdir)/PreferencesWindowController.m

doscript.o: $(srcdir)/doscript.m
	$(CC) $(CFLAGS) -o $@ -c $(srcdir)/doscript.m

main.o: $(srcdir)/main.m
	$(CC) $(CFLAGS) -o $@ -c $(srcdir)/main.m

Python\ Launcher: $(OBJECTS)
	$(CC) $(LDFLAGS) -o "Python Launcher" $(OBJECTS) -framework AppKit -framework Carbon

Info.plist: $(srcdir)/Info.plist.in
	sed 's/%VERSION%/'"`$(RUNSHARED) $(BUILDPYTHON) -c 'import platform; print(platform.python_version())'`"'/g' < $(srcdir)/Info.plist.in > Info.plist
