bin_PROGRAMS = suba
suba_SOURCES = suba.c
STAG = `git describe | awk -F[-.] '{printf("%s-%d.%d.%d\n",$$1,$$2,$$3,$$4+1);}'`

.ONESHELL:

xrlog:
	cd testdir1
	pwd

package:
	sudo checkinstall --install=no --pkgversion=1.5 --pkgname $(bin_PROGRAMS)

stag:
	git tag -a "$(STAG)" -m "version upgrade to $(STAG)"

stagpush:
	git push --tags
