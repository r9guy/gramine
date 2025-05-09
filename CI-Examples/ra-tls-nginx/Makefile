# Copyright (C) 2023 Gramine contributors
# SPDX-License-Identifier: BSD-3-Clause

ARCH_LIBDIR ?= /lib/$(shell $(CC) -dumpmachine)

ifeq ($(DEBUG),1)
GRAMINE_LOG_LEVEL = debug
else
GRAMINE_LOG_LEVEL = error
endif

.PHONY: all
all: nginx.manifest.sgx nginx.sig

%.manifest: %.manifest.template
	gramine-manifest \
		-Dentrypoint=$$(command -v gramine-ratls) \
		-Dlog_level=$(GRAMINE_LOG_LEVEL) \
		-Darch_libdir=$(ARCH_LIBDIR) \
		$< > $@

%.manifest.sgx %.sig: sgx-sign-%
	@:

.INTERMEDIATE: sgx-sign-%
sgx-sign-%: %.manifest
	gramine-sgx-sign \
		--manifest $< \
		--output $<.sgx

.PHONY: check
check: all
	@./run.sh

.PHONY: clean
clean:
	$(RM) -r \
		*.manifest \
		*.manifest.sgx \
		*.sig
