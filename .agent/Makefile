THREADS ?= $(shell nproc)
ifeq ($(shell [ $(THREADS) -gt 8 ] && echo yes),yes)
THREADS := 8
endif

.PHONY: all clones po6_clone e_clone busybee_clone po6 e busybee Replicant

all: Replicant

clones: po6_clone e_clone busybee_clone

po6_clone:
	@if [ -d po6/.git ]; then echo "Using existing clone in po6"; else git clone https://github.com/AaronFriel/po6.git po6; fi

po6: po6_clone
	cd po6 && autoreconf -i && ./configure && make -j$(THREADS) && make install && ldconfig

e_clone:
	@if [ -d e/.git ]; then echo "Using existing clone in e"; else git clone https://github.com/AaronFriel/e.git e; fi

e: po6 e_clone
	cd e && autoreconf -i && ./configure && make -j$(THREADS) && make install && ldconfig

busybee_clone:
	@if [ -d busybee/.git ]; then echo "Using existing clone in busybee"; else git clone https://github.com/AaronFriel/busybee.git busybee; fi

busybee: e busybee_clone
	cd busybee && autoreconf -i && ./configure && make -j$(THREADS) && make install && ldconfig

Replicant: busybee
	cd .. && autoreconf -i
	cd .. && ./configure --prefix=/opt/Replicant
	$(MAKE) -C .. -j$(THREADS)
	$(MAKE) -C .. check
