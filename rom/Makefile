# Simple makefile to snarf the Apple SuperSerialCard ROM image and patch it
# for ADTPro bootstrapping

ROM_SOURCE = https://mirrors.apple2.org.za/Apple%20II%20Documentation%20Project/Interface%20Cards/Serial/Apple%20II%20Super%20Serial%20Card/ROM%20Images/Apple%20II%20Super%20Serial%20Card%20ROM%20-%20341-0065-A.bin

all: 341-0065.bin adtpro-patch-341-0065.bin

adtpro-patch-341-0065.bin: adtpro-patch-341-0065.xdelta
	xdelta3 -d -s 341-0065.bin adtpro-patch-341-0065.xdelta \
		adtpro-patch-341-0065.bin

341-0065.bin:
	curl -o 341-0065.bin $(ROM_SOURCE)

clean:
	rm -f 341-0065.bin adtpro-patch-341-0065.bin
