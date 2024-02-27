# Makefile for ARSDK wireshark plugin
PLUGIN	:= arsdk.so
OBJS	:= packet-arsdk.o

TREE	:= tree.h
GEN	:= ../../../out/arsdk-native/staging-host/usr/lib/arsdkgen/arsdkgen.py
ARC_GEN := ../Tools/libARCommandsgen.py
INC	:= -I ../Includes \
			-I ../Sources \
			-I ../gen/Includes \
			-I /usr/local/include \
			-I ../../../out/arsdk-native/staging/usr/include/ \
			-I ../../libARNetworkAL/Includes \
			-I ../../libARSAL/Config/darwin \
			-I /Users/smclark/repos/wireshark/wireshark-4.2.3 \
			-I /opt/homebrew/Cellar/wireshark/4.2.3/include/wireshark \
			-I /opt/homebrew/Cellar/glib/2.78.4/include/glib-2.0 -I /opt/homebrew/Cellar/glib/2.78.4/lib/glib-2.0/include \
			-I /Users/smclark/repos/samba/librpc/ndr \
			-I /Users/smclark/repos/samba/lib/talloc \
			-I /Users/smclark/repos/samba/lib \
			-I /Users/smclark/repos/samba \
			-I /Users/smclark/repos/samba/bin/default/

WARN	:= -Wall -W -Wextra -Wdeclaration-after-statement -Wendif-labels \
	-Wpointer-arith -Wno-pointer-sign -Warray-bounds -Wformat-security \
	-Wold-style-definition -Wstrict-prototypes -Wjump-misses-init -Wvla \
	-Waddress -Wattributes -Wdiv-by-zero -Wignored-qualifiers -Wpragmas \
	-Wno-overlength-strings -Wwrite-strings -Wno-long-long -Wlogical-op

CFLAGS	:= -g -O2 $(WARN) $(INC) -fPIC -DPIC -DSTANDALONE_BUILD \
	-DWS_BUILD_DLL \
	-fexcess-precision=fast -fvisibility=hidden -fwrapv \
	-fno-strict-overflow -fno-delete-null-pointer-checks

LDFLAGS	:= -shared -fPIC -DPIC
STRIP_ARGS :=

UNAME_S := $(shell uname -s)
# if platform is on MacOS
ifeq ($(UNAME_S),Darwin)
    LDFLAGS += \
					-L /Users/smclark/repos/wireshark/wireshark-4.2.3/build/run \
					-L /usr/local/lib \
					-lwireshark -lwsutil -l smcdebug \
					-L/opt/homebrew/Cellar/glib/2.78.4/lib \
					-L/opt/homebrew/opt/gettext/lib \
					-lglib-2.0 -lintl
    STRIP_ARGS += -x
endif
# if platform is on Linux
ifeq ($(UNAME_S),Linux)
    LDFLAGS += -Wl,-soname -Wl,$(PLUGIN)
endif

all: $(PLUGIN)

# don't bother handling dependencies here
.PHONY: .FORCE

$(TREE): .FORCE
	python3 $(GEN) $(ARC_GEN) tree

%.o: %.c $(TREE) .FORCE
	@echo "CC $@"
	@$(CC) -c $< $(CFLAGS) -o $@

$(PLUGIN): $(OBJS)
	@echo "CCLD $@"
	@$(CC) $(LDFLAGS) $^ -o $@

#	@echo "STRIP $@"
#	@strip $(STRIP_ARGS) $@

clean:
	-rm -f $(OBJS) $(TREE) $(PLUGIN)
