bin_PROGRAMS = suba
suba_SOURCES = suba.c
STAG := $(shell git describe | awk -F[-.] '{printf("%s-%d.%d.%d\n",$$1,$$2,$$3,$$4+1);}')

package:
	sudo checkinstall --install=no --pkgname $(bin_PROGRAMS)

stag:
	git tag -a "$$STAG" -m "version upgrade to $$STAG"

stagpush:
	git push --tags
