###########################################################

ifeq (,$(PLATFORM))
PLATFORM=$(UNION_PLATFORM)
endif

ifeq (,$(PLATFORM))
$(error please specify PLATFORM, eg. PLATFORM=trimui make)
endif

###########################################################

REQUIRES_COMMANDER=other/DinguxCommander
REQUIRES_UNZIP60=other/unzip60

all: readmes
	cd show && make

early: $(REQUIRES_COMMANDER) $(REQUIRES_UNZIP60)
	mkdir -p other
	cd other/DinguxCommander && make -j
	cd other/unzip60 && make -f unix/Makefile.trimuismart unzip

clean:
	cd show && make clean
	cd other/DinguxCommander && make clean
	cd other/unzip60 && make -f unix/Makefile.trimuismart clean
	
$(REQUIRES_COMMANDER):
	git clone --depth 1 --branch trimui-smart https://github.com/shauninman/DinguxCommander.git $(REQUIRES_COMMANDER)
	cd $(REQUIRES_COMMANDER) && git checkout f86496b176779632b806e3f821c97ee19c7da99a
$(REQUIRES_UNZIP60):
	git clone --depth 1 https://github.com/shauninman/unzip60.git $(REQUIRES_UNZIP60)
	cd $(REQUIRES_UNZIP60) && git checkout f6389e3aafc0fc0d31209f7f2a33c4392f92342a

include ../all/readmes/makefile