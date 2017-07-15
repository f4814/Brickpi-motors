CC = avr-gcc
OBJCOPY = avr-objcopy

TARGET = main
OBJECTS = motors.o main.o uart.o

## Chip stuff
MCU = atmega328p
F_CPU = 16000000

## Compiler/linker options 
CFLAGS = -Os -g -std=gnu99 -Wall 
CFLAGS += -ffunction-sections -fdata-sections
CFLAGS += -DF_CPU=$(F_CPU)

TARGET_ARCH = -mmcu=$(MCU)

LDFLAGS = -Wl,-Map,$(TARGET).map 
LDFLAGS += -Wl,--gc-sections 

all: flash

.PHONY: flash
flash: $(TARGET).hex
	sudo avrdude -P usb -c usbtiny -p m328p -U flash:w:$<
	
%.hex: %.elf
	$(OBJCOPY) -j .text -j .data -O ihex $< $@

%.elf: $(OBJECTS)
	$(CC) $(LDFLAGS) $(TARGET_ARCH) $^ -o $@

clean:
	rm -f $(TARGET).elf $(TARGET).hex $(TARGET).map

build:
	mkdir bin build

.PHONY: flash-fuse
flash-fuse:
	sudo avrdude -P usb -c usbtiny -p m328p -U lfuse:w:0xFF:m
	sudo avrdude -P usb -c usbtiny -p m328p -U hfuse:w:0xDA:m
	sudo avrdude -P usb -c usbtiny -p m328p -U efuse:w:0x05:m
