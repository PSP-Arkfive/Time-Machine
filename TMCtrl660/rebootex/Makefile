PSPSDK=$(shell psp-config --pspsdk-path)
ARKSDK = ../../../ark-dev-sdk
BOOTLOADEX = ../../../BootLoadEx
PYTHON=$(shell which python3)

TARGET = reboot.bin.gz

all: $(TARGET)

INCDIR = $(PSPSDK)/include $(ARKSDK)/include $(ARKSDK)/include/iplsdk
CFLAGS = -std=c99 -Wall -Os -G0 -fno-pic $(addprefix -I, $(INCDIR)) -DREBOOTEX -DMS_IPL -DFAT_DEBUG

CC = psp-gcc
LD = psp-ld
STRIP = psp-strip
OBJCOPY = psp-objcopy
LINKFILE = $(BOOTLOADEX)/linkfile.l

REBOOT_OBJS = \
	main.o \
	lib.o \
	pspbtcnf.o \
	scanner.o \


LIBS = -L $(ARKSDK)/libs  -liplsdk

ifdef DEBUG
CFLAGS += -DDEBUG=$(DEBUG)
LIBS += -lcolordebugger
endif

crt0.o : $(BOOTLOADEX)/crt0.S
	$(CC) -I$(PSPSDK)/include -c $< -o crt0.o

$(REBOOT_OBJS) : %.o : $(BOOTLOADEX)/%.c
	$(CC) $(CFLAGS) -c $< -o $@

patches.o: $(BOOTLOADEX)/psp_rebootex/patches.c
	$(CC) $(CFLAGS) -I$(BOOTLOADEX) -c $< -o $@

main.elf: crt0.o $(REBOOT_OBJS) $(C_OBJS) patches.o

$(TARGET): main.elf
	$(Q)$(STRIP) -s $<
	$(Q)$(OBJCOPY) -O binary $< $(patsubst %.gz,%,$(TARGET))
	$(Q)$(PYTHON) $(BOOTLOADEX)/gz.py $(patsubst %.gz,%,$(TARGET)) $@
	$(Q)bin2c $@ payload.h rebootbuffer_ms_psp
	@echo GET $@

clean:
	$(Q)rm -rf *~ *.s *.o *.elf reboot.bin reboot.bin.gz payload.h $(EXTRA_CLEAN)

include $(BOOTLOADEX)/beauty_bin.mak
