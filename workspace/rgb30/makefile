###########################################################

ifeq (,$(PLATFORM))
PLATFORM=$(UNION_PLATFORM)
endif

ifeq (,$(PLATFORM))
$(error please specify PLATFORM, eg. PLATFORM=rgb30 make)
endif

###########################################################

REQUIRES_COMMANDER=other/DinguxCommander
REQUIRES_SDLCOMPAT=other/sdl12-compat

all: readmes
	cd show && make
	
early: $(REQUIRES_COMMANDER) $(REQUIRES_SDLCOMPAT)/build
	cd $(REQUIRES_COMMANDER) && make -j
	cd $(REQUIRES_SDLCOMPAT) && cmake --build build

clean:
	cd $(REQUIRES_COMMANDER) && make clean
	cd $(REQUIRES_SDLCOMPAT) && rm -rf build

###########################################################

$(REQUIRES_COMMANDER):
	git clone --depth 1 --branch minui-rgb30 https://github.com/shauninman/DinguxCommander.git $(REQUIRES_COMMANDER)
	cd $(REQUIRES_COMMANDER) && git checkout b60f06a5b92fa3a1551dc618776b103059cf25a2

$(REQUIRES_SDLCOMPAT):
	git clone --depth 1 --branch minui-rgb30 https://github.com/shauninman/sdl12-compat.git $(REQUIRES_SDLCOMPAT)
	cd $(REQUIRES_SDLCOMPAT) && git checkout 411bf7145449af2b296f28a4881b8c4a75a3a68b

$(REQUIRES_SDLCOMPAT)/build: $(REQUIRES_SDLCOMPAT)
	cd $(REQUIRES_SDLCOMPAT) && cmake --toolchain ./toolchain.cmake -Bbuild -DCMAKE_BUILD_TYPE=Release .

include ../all/readmes/makefile
