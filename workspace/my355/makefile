# my355

###########################################################

ifeq (,$(PLATFORM))
PLATFORM=$(UNION_PLATFORM)
endif

ifeq (,$(PLATFORM))
$(error please specify PLATFORM, eg. PLATFORM=trimui make)
endif

###########################################################

REQUIRES_EVTEST=other/evtest
REQUIRES_MKBOOTIMG=other/mkbootimg
REQUIRES_RSCEGO=other/rsce-go
REQUIRES_COMMANDER=other/DinguxCommander-sdl2

all: readmes
	cd show && make
	cd other/squashfs && make

early: $(REQUIRES_EVTEST) $(REQUIRES_MKBOOTIMG) $(REQUIRES_RSCEGO) $(REQUIRES_COMMANDER)
	mkdir -p other
	cd $(REQUIRES_COMMANDER) && make -j
	cd $(REQUIRES_EVTEST) && $(CROSS_COMPILE)gcc -o evtest evtest.c
	cd $(REQUIRES_MKBOOTIMG) && make
	cd $(REQUIRES_RSCEGO) && go build -o rsce_tool
	
clean:
	cd show && make clean
	cd $(REQUIRES_COMMANDER) && make clean

###########################################################

$(REQUIRES_COMMANDER):
	git clone --depth 1 https://github.com/shauninman/DinguxCommander-sdl2.git $(REQUIRES_COMMANDER)
	cd $(REQUIRES_COMMANDER) && git checkout 8eaa0efa132a6eae909df5aa1b31950044f568cc
$(REQUIRES_EVTEST):
	git clone --depth 1 https://github.com/freedesktop-unofficial-mirror/evtest.git $(REQUIRES_EVTEST)
	cd $(REQUIRES_EVTEST) && git checkout b8343ec1124da18bdabcc04809a8731b9e39295d
$(REQUIRES_MKBOOTIMG):
	git clone --depth 1 https://github.com/osm0sis/mkbootimg $(REQUIRES_MKBOOTIMG)
	cd $(REQUIRES_MKBOOTIMG) && git checkout 17cea80bd5af64e45cdf9e263cad7555030e0e86
$(REQUIRES_RSCEGO):
	git clone https://github.com/Evsio0n/rsce-go $(REQUIRES_RSCEGO)
	cd $(REQUIRES_RSCEGO) && git checkout 7aa3ebb91eb79b591e1442e6fd287efd24cc6cb1
	cd $(REQUIRES_RSCEGO) && go mod tidy && go mod download

include ../all/readmes/makefile
