
AUTOMAKE_OPTIONS=foreign

EXTRA_DIST = $(shell find . -type d \( -name ".??*" -o -name "complied" -o -name "node_modules" \) -prune -o -not -name ".*" -a -not -name "*~" -a -not -name "*.tmp"  -a -type f -print )

if BUILD_QOOXDOO_APP

all-local: build

nm: node_modules/.bin/qx package.json

node_modules/.bin/qx:
	$(NPM) i

source: nm $(EXTRA_DIST)
	CALLBACKERY_QX=$(CALLBACKERY_QX) $(NPX) qx compile --feedback=false --erase

source-watch: nm $(EXTRA_DIST)
	CALLBACKERY_QX=$(CALLBACKERY_QX) $(NPX) qx compile --watch --feedback=false --erase

build: nm ../public/versions.json

clean: nm
	$(NPX) qx clean

../public/versions.json: $(EXTRA_DIST)
	CALLBACKERY_QX=$(CALLBACKERY_QX) $(NPX) qx compile --target=build --feedback=false --erase

endif
