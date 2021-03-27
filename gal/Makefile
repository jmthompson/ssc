GALASM := galasm

SRCS      := $(wildcard *.pld)
JEDFILES  := ${SRCS:.pld=.jed}
CHIPFILES := ${SRCS:.pld=.chp}
FUSEFILES := ${SRCS:.pld=.fus}
PINFILES  := ${SRCS:.pld=.pin}

all: ${JEDFILES}

clean:
	rm -f ${JEDFILES} ${CHIPFILES} ${FUSEFILES} ${PINFILES}

%.jed: %.pld
	${GALASM} $<
