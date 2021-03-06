#
# This file is subject to the terms and conditions of the GNU General Public
# License.  See the file "COPYING" in the main directory of this archive
# for more details.
#
# Copyright (C) 1995, 1998, 1999 by Ralf Baechle
# Copyright (C) 2004  Maciej W. Rozycki
#

USE_STANDARD_AS_RULE := true

#
# Some DECstations need all possible sections of an ECOFF executable
#
ifdef CONFIG_DECSTATION
  E2EFLAGS = -a
else
  E2EFLAGS =
endif

#
# Drop some uninteresting sections in the kernel.
# This is only relevant for ELF kernels but doesn't hurt a.out
#
drop-sections	= .reginfo .mdebug .comment .note .pdr .options .MIPS.options
strip-flags	= $(addprefix --remove-section=,$(drop-sections))

VMLINUX = $(TOPDIR)/vmlinux

all:

boot: vmlinux.ecoff vmlinux.srec addinitrd

vmlinux.ecoff: $(CONFIGURE) elf2ecoff $(VMLINUX)
	./elf2ecoff $(VMLINUX) vmlinux.ecoff $(E2EFLAGS)

elf2ecoff: $(TOPDIR)/arch/mips/boot/elf2ecoff.c
	$(HOSTCC) -I$(TOPDIR)/arch/mips/boot -I- -o $@ $^

vmlinux.srec: $(CONFIGURE) $(VMLINUX)
	$(OBJCOPY) -S -O srec $(strip-flags) $(VMLINUX) vmlinux.srec

addinitrd: $(TOPDIR)/arch/mips/boot/addinitrd.c
	$(HOSTCC) -I$(TOPDIR)/arch/mips/boot -I- -o $@ $^

# Don't build dependencies, this may die if $(CC) isn't gcc
dep:

clean:
	rm -f vmlinux.ecoff vmlinux.srec

mrproper:
	rm -f vmlinux.ecoff vmlinux.srec addinitrd elf2ecoff

dummy:

include $(TOPDIR)/Rules.make
