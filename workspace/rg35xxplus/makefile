#rg35xx

###########################################################

ifeq (,$(PLATFORM))
PLATFORM=$(UNION_PLATFORM)
endif

ifeq (,$(PLATFORM))
$(error please specify PLATFORM, eg. PLATFORM=trimui make)
endif

###########################################################

REQUIRES_UNZIP60=other/unzip60
REQUIRES_SDL2=other/sdl2
REQUIRES_DTC=other/dtc
REQUIRES_FBSET=other/fbset

all: readmes
	cd boot && ./build.sh
	cd init && make
	cd show && make

early: $(REQUIRES_UNZIP60) $(REQUIRES_SDL2) $(REQUIRES_DTC) $(REQUIRES_FBSET)
	mkdir -p other
	cd $(REQUIRES_DTC) && CC=${CROSS_COMPILE}gcc make NO_PYTHON=1 dtc
	cd $(REQUIRES_UNZIP60) && make -f unix/Makefile.trimuismart unzip
	cd $(REQUIRES_FBSET) && make
	cd $(REQUIRES_SDL2) && CFLAGS="-O2 -march=armv8-a+crc -mtune=cortex-a53 -mcpu=cortex-a53 -ffast-math -ftree-vectorize" \
		./configure CC=${CROSS_COMPILE}gcc --host=arm-buildroot-linux-gnueabihf \
		  --enable-arm-neon \
		  --disable-oss \
		  --disable-rpath \
		  --disable-arts \
		  --disable-esd \
		  --disable-dbus \
		  --disable-diskaudio \
		  --disable-dummyaudio \
		  --disable-pulseaudio \
		  --disable-video-kmsdrm \
		  --disable-video-vivante \
		  --disable-video-cocoa \
		  --disable-video-metal \
		  --disable-video-dummy \
		  --disable-video-offscreen \
		  --disable-video-x11 \
		  --disable-video-vulkan \
		  --disable-ime \
		  --disable-ibus \
		  --disable-fcitx \
		  --disable-joystick-mfi \
		  --disable-directx \
		  --disable-xinput \
		  --disable-wasapi \
		  --disable-hidapi-joystick \
		  --disable-hidapi-libusb \
		  --disable-joystick-virtual \
		  --enable-video-mali \
		  --disable-libsamplerate && make
	# copy libs from toolchain (later moved to build)
	cp $(PREFIX)/lib/libSDL2_image-2.0.so.0.0.1 ../libSDL2_image-2.0.so.0
	cp $(PREFIX)/lib/libSDL2_ttf-2.0.so.0.14.0 ../libSDL2_ttf-2.0.so.0
	
clean:
	cd boot && rm -rf output
	cd $(REQUIRES_UNZIP60) && make -f unix/Makefile.trimuismart clean
	cd $(REQUIRES_FBSET) && make clean
	
$(REQUIRES_UNZIP60):
	git clone --depth 1 https://github.com/shauninman/unzip60.git $(REQUIRES_UNZIP60)
	cd $(REQUIRES_UNZIP60) && git checkout f6389e3aafc0fc0d31209f7f2a33c4392f92342a

$(REQUIRES_SDL2):
	git clone https://github.com/JohnnyonFlame/SDL-malifbdev-rot $(REQUIRES_SDL2)
	cd $(REQUIRES_SDL2) && git checkout d4a7d7503524cc469fe775242f3f925d4dd56c88

$(REQUIRES_DTC):
	git clone https://github.com/dgibson/dtc $(REQUIRES_DTC)
	cd $(REQUIRES_DTC) && git checkout 7f3184a6c550bb8fb59e93c9901d75dced889dcf

$(REQUIRES_FBSET):
	git clone https://github.com/shauninman/union-fbset $(REQUIRES_FBSET)
	cd $(REQUIRES_FBSET) && git checkout f262058ae46e628207f29c4813324ad4dc992fbd

include ../all/readmes/makefile
