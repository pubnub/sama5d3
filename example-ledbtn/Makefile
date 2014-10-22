CUSTOM_CFLAGS=-Wall -ggdb3 -O3
SYS_CFLAGS=-std=gnu99 `pkg-config --cflags libpubnub json`
LIBS=`pkg-config --libs libpubnub json`

OBJS=sama5d3-ledbtn.o

all: sama5d3-ledbtn

sama5d3-ledbtn: sama5d3-ledbtn.o
	$(call cmd,link)

clean:
	rm -f *.o sama5d3-ledbtn

install:

-include Makefile.lib
