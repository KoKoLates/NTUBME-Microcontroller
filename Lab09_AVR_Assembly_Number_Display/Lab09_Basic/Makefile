
# compile
MCU_compiler=atmega328p
COMPILE_CCCFLAGS=-c -std=c11 -Wall -Os -ffunction-sections -fdata-sections -mmcu=${MCU_compiler} -DF_CPU=${F_CPU}
LINK_CCCFLAGS=-Os -mmcu=${MCU_compiler} -ffunction-sections -fdata-sections -Wl,--gc-sections
CCC=avr-gcc
F_CPU=8000000UL

# flash
USB_PORT=/dev/ttyS6
MCU_dude=m328p
OBJCOPY=avr-objcopy
PROGRAMMER=stk500v1 # ArduinoISP
PROGRAMMER_BAUD=19200 # 19200 for ArduinoISP stk500v1

TARGET=main
SOURCES := main
OBJS := $(patsubst %,%.o,$(SOURCES))

all: link flash

.PHONY: link
link: $(OBJS)
	$(CCC) ${LINK_CCCFLAGS} $^ -o ${TARGET}.elf

%.o: %.S
	${CCC} ${COMPILE_CCCFLAGS} $< -o $@

.PHONY: flash
flash:
	${OBJCOPY} -O ihex -R .eeprom ${TARGET}.elf ${TARGET}.ihex
	avrdude -p ${MCU_dude} -c ${PROGRAMMER} -b ${PROGRAMMER_BAUD} -P ${USB_PORT} -U flash:w:${TARGET}.ihex:i

.PHONY: clean
clean:
	-rm -f *.o *.ihex *.elf
