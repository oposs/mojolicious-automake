#  Copyright (C) 2019 Tobias Oetiker

AUTOMAKE_OPTIONS =  foreign

THIRDPARTY_DIR := $(shell pwd)

THIRDPARTY_DIST := $(shell test -d cache && find cache -type f )

EXTRA_DIST = $(THIRDPARTY_DIST) $(wildcard bin/cpanm)

all-local: touch

touch: cache/modules/02packages.details.txt.gz bin/cpanm
	$(AM_V_GEN)echo "Installing Dependencies from Cache using cpanm"
	PERL_CPANM_HOME=$(THIRDPARTY_DIR) $(PERL) bin/cpanm -q --notest --local-lib-contained $(THIRDPARTY_DIR) --from file://$(THIRDPARTY_DIR)/cache --installdeps ..
	$(AM_V_GEN)touch touch

bin/cpanm:
	$(AM_V_GEN)mkdir -p bin
	$(URL_CAT) https://cpanmin.us > bin/cpanm
	$(AM_V_GEN)chmod 755 bin/cpanm

cache/modules/02packages.details.txt.gz: ../cpanfile
	$(AM_V_GEN)echo "Installing Dependencies using Carton"
	test -d carton || $(PERL) bin/cpanm -q --notest --local-lib-contained $(THIRDPARTY_DIR)/carton Carton
	PERL5LIB=$(THIRDPARTY_DIR)/carton/lib/perl5 PERL_CARTON_PATH=$(THIRDPARTY_DIR) $(PERL) $(THIRDPARTY_DIR)/carton/bin/carton install $(shell test -f ../cpanfile.snapshot && echo --deployment)
	PERL5LIB=$(THIRDPARTY_DIR)/carton/lib/perl5 PERL_CARTON_PATH=$(THIRDPARTY_DIR) $(PERL) $(THIRDPARTY_DIR)/carton/bin/carton bundle
	rm -rf cache
	mv ../vendor/cache .

update: cache/touch
	$(AM_V_GEN)PERL5LIB=$(THIRDPARTY_DIR)/carton/lib/perl5 PERL_CARTON_PATH=$(THIRDPARTY_DIR) $(PERL) $(THIRDPARTY_DIR)/carton/bin/carton update

clean-local:
	ls -1 | grep -v Makefile | grep -v bin | grep -v cache | xargs rm -rf

distclean-local:
	ls -1 | grep -v Makefile | xargs rm -rf

install-exec-hook:
	cp -fr lib/perl5/* $(DESTDIR)$(libdir)
