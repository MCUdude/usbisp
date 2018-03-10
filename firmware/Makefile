#
#   Makefile for usbasp
#   20061119   Thomas Fischl        original
#   20061120   Hanns-Konrad Unger   help: and MCU=atmega48 added
#   20180310   MCUdude              re-write makefile
#


# The MX-USBISP-V4.00 clone is based on ATmega88, but may be compiled for other ATmegas as well
MCU ?= atmega88
HFUSE ?= 0xDD
LFUSE ?= 0xEF
CLOCK ?= 12000000

ISP ?= usbtiny
PORT= -Pusb

help:
	@echo "Usage:"
	@echo "       make                           Same as make help"
	@echo "       make help                      Same as make\n"
	
	@echo "       make hex                       Create hex for ATmega88"
	@echo "       make clean_build               Clean build for ATmega88"
	@echo "       make disasm                    Disasm hex file for ATmega88"
	@echo "       make fuses                     Program fuses to ATmega88"
	@echo "       make flash                     Upload hex file to ATmega88\n"
	
	@echo "       make hex MCU=atmegaXX          Create hex for ATmegaXX"
	@echo "       make clean_build MCU=atmegaXX  Clean build for ATmegaXX"
	@echo "       make disasm MCU=atmegaXX       Disasm hex file for ATmegaXX"
	@echo "       make fuses MCU=atmegaXX        Program fuses to ATmegaXX"
	@echo "       make flash MCU=atmegaXX        Upload hex file to ATmegaXX\n"

	@echo "Current values:"
	@echo "       MCU   = ${MCU}"
	@echo "       LFUSE = ${LFUSE}"
	@echo "       HFUSE = ${HFUSE}"
	@echo "       CLOCK = ${CLOCK}"
	@echo "       ISP   = ${ISP}"

COMPILE = avr-gcc -Wall -O2 -Isrc/usbdrv -Isrc/. -mmcu=$(MCU) -DF_CPU=$(CLOCK) # -DDEBUG_LEVEL=2

OBJECTS = src/usbdrv/usbdrv.o src/usbdrv/usbdrvasm.o src/usbdrv/oddebug.o src/isp.o src/clock.o src/tpi.o src/main.o

.c.o:
	$(COMPILE) -c $< -o $@
#-Wa,-ahlms=$<.lst

.S.o:
	$(COMPILE) -x assembler-with-cpp -c $< -o $@
# "-x assembler-with-cpp" should not be necessary since this is the default
# file type for the .S (with capital S) extension. However, upper case
# characters are not always preserved on Windows. To ensure WinAVR
# compatibility define the file type manually.

.c.s:
	$(COMPILE) -S $< -o $@

clean_build:
	rm -f $(MCU)/usbasp_$(MCU).bin
	rm -f src/usbasp_$(MCU).lst
	rm -f src/usbasp_$(MCU).obj
	rm -f src/usbasp_$(MCU).cof
	rm -f src/usbasp_$(MCU).list
	rm -f src/usbasp_$(MCU).map
	rm -f src/usbasp_$(MCU).eep.hex
	rm -f src/*.o
	rm -f src/usbasp_$(MCU).s
	rm -f src/usbdrv/*.o

# file targets:
bin:	$(OBJECTS)
	$(COMPILE) -o $(MCU)/usbasp_$(MCU).bin $(OBJECTS)

hex:	bin
	rm -f $(MCU)/usbasp_$(MCU).hex usbasp_$(MCU).eep.hex
	avr-objcopy -j .text -j .data -O ihex $(MCU)/usbasp_$(MCU).bin $(MCU)/usbasp_$(MCU).hex

disasm:	$(MCU)/usbasp_$(MCU).bin
	avr-objdump -d $(MCU)/usbasp_$(MCU).bin

cpp:
	$(COMPILE) -E src/main.c

flash:
	avrdude -c ${ISP} $(PORT) -p ${MCU} -U flash:w:$(MCU)/usbasp_$(MCU).hex

fuses:
ifeq ($(MCU), atmega8)
	avrdude -c ${ISP} $(PORT) -p ${MCU} -u -U hfuse:w:0xC9:m -U lfuse:w:$(LFUSE):m
else
	avrdude -c ${ISP} $(PORT) -p ${MCU} -u -U hfuse:w:$(HFUSE):m -U lfuse:w:$(LFUSE):m
endif
	
	
	




