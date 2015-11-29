COMMIT_REV ?= $(shell echo)
KERNEL_SOURCE_VERSION ?= $(shell uname -r)
KERNEL_TREE ?= /lib/modules/$(KERNEL_SOURCE_VERSION)/build
EXTRA_CFLAGS += -I$(KERNEL_TREE)/drivers/md -I./ -DCOMMIT_REV="\"$(COMMIT_REV)\""
EXTRA_CFLAGS += -I$(KERNEL_TREE)/include/ -I$(KERNEL_TREE)/include/linux

obj-m := eprd.o
PWD := $(shell pwd)
CC := gcc -O2
default:
	$(MAKE) -C $(KERNEL_TREE) SUBDIRS=$(PWD) modules
	$(CC) eprd_setup.c -o eprd_setup
	$(CC) tools/writetest.c -o tools/writetest

.PHONY: all
all: modules
.PHONY:    modules
modules:
	$(MAKE) -C $(KERNEL_TREE) SUBDIRS=$(PWD) modules
	$(CC) eprd_setup.c -o eprd_setup
.PHONY: modules_install
modules_install: modules
	install -o root -g root -m 0755 -d $(DESTDIR)/lib/modules/$(KERNEL_SOURCE_VERSION)/extra/eprd/
	install -o root -g root -m 0755 eprd.ko $(DESTDIR)/lib/modules/$(KERNEL_SOURCE_VERSION)/extra/eprd/
	depmod -a
.PHONY: install
install: modules_install
.PHONY: clean
clean:
	rm -f *.c~ *.o *.ko eprd.mod.c modules.order Module.symvers .*.cmd
	rm -rf .tmp_versions
	rm -f tools/writetest
	rm -f eprd.ko.unsigned
	rm -f eprd_setup
