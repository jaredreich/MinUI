###########################################################

ifeq (,$(PLATFORM))
PLATFORM=$(UNION_PLATFORM)
endif

ifeq (,$(PLATFORM))
	$(error please specify PLATFORM, eg. PLATFORM=trimui make)
endif

###############################

# overrides shared by all cores

# Pin all cores to commits from no later than Nov 26, 2025
# so builds are reproducible and not broken by upstream changes.
# Platform-specific _HASH values (set before this include) take precedence.
fceumm_HASH ?= 4070514256875f83edecbedc6d52eac58d87bc60
gambatte_HASH ?= 6924c76ba03dadddc6e97fa3660f3d3bc08faa94
gpsp_HASH ?= 6250979f01db3f9ee5ebca179316e1319f017b48
pcsx_rearmed_HASH ?= 3b48e2876ab1ca6300acad223606eeb59f670d8f
picodrive_HASH ?= ec68ce858330cb1f90ece789f2191d82b5bec833
snes9x2005_plus_HASH ?= 74d871db9b4dba6dbe6c5ecebc88cbf255be5349
mednafen_pce_fast_HASH ?= 7e9b257b8a591cb7e00f9e55371edba19db9799c
mednafen_vb_HASH ?= 8f837ebc077afdd6652efb2827fd8308a07113ca
fake-08_HASH ?= 288268d44c674a968d1bc083bb86375124de4a3f
mednafen_supafaust_HASH ?= f1c23ef20f8e9e4f6a1bdb3fa79d9841a8366598
mgba_HASH ?= c758314a639aa0066e7b65a8341448181b73c804
pokemini_HASH ?= 78656d4615691c393e65e48672ce5fff5b1f97d3
race_HASH ?= 171950ea64c96ead503340ed60fa7ba18f56325c

###############################

# this logic was broken out from picoarch's all-in-one makefile

PATCH = git apply
PROCS = -j4

###############################

BUILD_ROOT=../../$(PLATFORM)/cores

define TEMPLATE=

$1_REPO ?= https://github.com/libretro/$(1)
$1_MAKE ?= make $(and $($1_MAKEFILE),-f $($1_MAKEFILE)) platform=$(PLATFORM) $($(1)_FLAGS)
$1_BUILD_PATH ?= $(1)

src/$(1):
	mkdir -p src
	cd src && git clone $(if $($1_HASH),,--depth 1) --recursive $$($(1)_REPO) $(1)
	$(if $($1_HASH),cd src/$$($1_BUILD_PATH) && git checkout $($1_HASH) && git submodule update --init --recursive && echo $($1_HASH),)

src/$(1)/.patched: src/$(1)
	(test ! -f patches/$(1).patch) || (test -f src/$(1)/.patched) || (cd src/$(1) && $(PATCH) -p1 < ../../patches/$(1).patch && touch .patched && true)

src/$(1)/.patched-all:
	(test ! -d ../../all/cores/patches/$(1)) || (test -f src/$(1)/.patched-all) || (cd src/$(1) && $(foreach patch, $(sort $(wildcard ../../all/cores/patches/$(1)/*.patch)), $(PATCH) -p1 < ../../$(patch) &&) touch .patched-all && true)
	

output/$(1)_libretro.so: src/$(1)/.patched src/$(1)/.patched-all
	mkdir -p output
	cd src/$$($1_BUILD_PATH) && $$($1_MAKE) $(PROCS)
	mv src/$$($1_BUILD_PATH)/$(if $($(1)_CORE),$($(1)_CORE),$(1)_libretro.so) ./output

clone-$(1): src/$(1)

status-$(1): src/$(1)
	cd src/$(1) && git show --oneline -s

patch-$(1): src/$(1)/.patched

clean-$(1):
	test ! -d src/$(1) || cd src/$$($1_BUILD_PATH) && $$($1_MAKE) clean
	rm -rf output/$(1)_libretro.so

$(1): output/$(1)_libretro.so

endef

###############################

all: cores

$(foreach CORE,$(CORES),$(eval $(call TEMPLATE,$(CORE))))

cores: $(foreach CORE,$(CORES),$(CORE))
	
clean: $(foreach CORE,$(CORES),clean-$(CORE))
	
nuke:
	rm -rf ./output
	rm -rf ./src