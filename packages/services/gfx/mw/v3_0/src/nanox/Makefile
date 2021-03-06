##############################################################################
# Microwindows template Makefile
# Copyright (c) 2000, 2002 Martin Jolicoeur, Greg Haerr
##############################################################################

ifndef TOP
`TOP = ..
CONFIG = $(TOP)/config
endif

include $(CONFIG)

VPATH := $(TOP)/nanox

######################## Additional Flags section ############################

# Directories list for header files
INCLUDEDIRS +=

# Defines for preprocessor
ifeq ($(LINK_APP_INTO_SERVER), Y)
DEFINES += -DNONETWORK=1
endif

# Compilation flags for C files OTHER than include directories
CFLAGS +=
# Preprocessor flags OTHER than defines
CPPFLAGS +=
# Linking flags
LDFLAGS +=

############################# targets section ################################

ifeq ($(NANOX), Y)

# If you want to create a library with the objects files, define the name here
LIBNAME = libnano-X.a
LIBNAMESO = libnano-X.so

# Nano-X server files
NANOXFILES = srvmain.o srvfunc.o srvutil.o srvevent.o srvclip.o
NETFILES = srvnet.o
UTILFILES = nxdraw.o nxutil.o

ifeq ($(LINK_APP_INTO_SERVER), Y)
NANOXFILES += $(UTILFILES)
OBJS = $(NANOXFILES)
else
NANOXFILES += $(NETFILES)
OBJS = client.o clientfb.o nxproto.o error.o $(UTILFILES)
ifeq ($(ARCH), ECOS) 
TARGET = $(BUILD)/bin/nano-X.o
else
TARGET = $(BUILD)/bin/nano-X
endif
all: default $(TARGET)
endif

endif

######################### Makefile.rules section #############################

include $(TOP)/Makefile.rules

######################## Tools targets section ###############################

# Server binary ...
ifeq ($(SHAREDLIBS), Y)
$(BUILD)/bin/nano-X: $(NANOXFILES) $(NANOXSERVERLIBS) $(TOP)/config
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $(NANOXFILES) $(CCNANOXSERVERLIBS)
else
$(BUILD)/bin/nano-X.o: $(OBJS) $(NANOXFILES) $(NANOXSERVERLIBS) $(TOP)/config
#	$(CC) $(CFLAGS) $(LDFLAGS) $(OBJS) $(NANOXFILES) $(NANOXSERVERLIBS) -Wl,-r -Wl,--retain-symbols-file -Wl,nanox.syms -Wl,--defsym -Wl,nanox_main=main -o XX.o
	$(CC) $(CFLAGS) $(LDFLAGS) $(OBJS) $(NANOXFILES) $(NANOXSERVERLIBS) -Wl,-r -Wl,--defsym -Wl,nanox_main=main -o XX.o
	$(NM) XX.o | grep -v _main | grep -v Gr | grep -v nx | grep ' T' | awk -f $(TOP)/ecos/retain.awk | xargs $(OBJCOPY) XX.o $@
#	rm -f XX.o

$(BUILD)/bin/nano-X: $(NANOXFILES) $(NANOXSERVERLIBS) $(TOP)/config
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $(NANOXFILES) $(NANOXSERVERLIBS) $(LDFLAGS)
endif
