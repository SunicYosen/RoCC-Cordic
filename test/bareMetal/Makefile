
BIT ?= 32
NAME ?= cordicrocc

GCC ?= riscv$(BIT)-unknown-elf-gcc
OBJCOPY ?= riscv$(BIT)-unknown-elf-objcopy
OBJDUMP ?= riscv$(BIT)-unknown-elf-objdump

all: ${NAME}.rv$(BIT) ${NAME}.bin$(BIT) ${NAME}.hex$(BIT) ${NAME}.dump$(BIT)

%.bin$(BIT): %.rv$(BIT)
	$(OBJCOPY) -O binary --change-addresses=-0x80000000 --only-section .text $< $@

%.rv$(BIT): %.S link.ld
	$(GCC) -Tlink.ld $< -nostdlib -static -o $@

%.dump$(BIT): %.rv$(BIT)
	$(OBJDUMP) -d $< > $@

%.hex$(BIT): %.bin$(BIT)
	xxd -e $< | xxd -r | xxd -ps | tr -d  "\n" | sed "s/$$/ffffffff/" > $@

.PHONY:clean

clean:
	rm ${NAME}.hex$(BIT) ${NAME}.bin$(BIT) ${NAME}.dump$(BIT) ${NAME}.rv$(BIT)
