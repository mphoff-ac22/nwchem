include ../../config/makefile.h

DFTD4_VERSION ?= 4.2.0
DFTD4_PREFIX = $(CURDIR)/install
DFTD4_LIBDIR = $(DFTD4_PREFIX)/lib
DFTD4_INCDIR = $(DFTD4_PREFIX)/include
DFTD4_MODULEDIR = $(CURDIR)/dftd4/build/include
DFTD4_ARCHIVES = $(DFTD4_LIBDIR)/libdftd4.a \
	$(DFTD4_LIBDIR)/libmulticharge.a $(DFTD4_LIBDIR)/libmctc-lib.a \
	$(DFTD4_LIBDIR)/libmstore.a
DFTD4_BRIDGE = $(LIBDIR)/libnwc_dftd4.so
DFTD4_STAMP = $(DFTD4_PREFIX)/.dftd4-$(DFTD4_VERSION)

all: $(DFTD4_BRIDGE)

$(DFTD4_STAMP): build_dftd4.sh
	./build_dftd4.sh $(DFTD4_VERSION) $(DFTD4_PREFIX)
	@touch $@

$(DFTD4_BRIDGE): nw_dftd4.c nw_dftd4_params.F90 nw_dftd4.map $(DFTD4_STAMP)
	$(CC) $(COPTIONS) -fPIC -I$(DFTD4_INCDIR) -c nw_dftd4.c -o nw_dftd4.o
	$(FC) $(FOPTIONS) -fPIC -I$(DFTD4_MODULEDIR) -c nw_dftd4_params.F90 -o nw_dftd4_params.o
	$(FC) -shared -o $@ nw_dftd4.o nw_dftd4_params.o -Wl,--whole-archive \
	  $(DFTD4_ARCHIVES) -Wl,--no-whole-archive -llapack -lblas \
	  -Wl,-Bsymbolic -Wl,--exclude-libs,ALL \
	  -Wl,--version-script=$(CURDIR)/nw_dftd4.map

clean:
	$(RM) nw_dftd4.o nw_dftd4_params.o $(DFTD4_BRIDGE)

realclean: clean
	$(RM) -r dftd4-* dftd4 install

64_to_32 32_to_64 sngl_to_dbl dbl_to_sngl dependencies include_stamp:
	@true
