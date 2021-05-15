bin_PROGRAMS = suba
suba_SOURCES = suba.c

package:
	sudo checkinstall --install=no --pkgname $(bin_PROGRAMS)

stag:
	STAG=`git describe | awk -F[-.] '{printf("%s-%d.%d.%d\n",$$1,$$2,$$3,$$4+1);}'` && \
	  git tag -a "$$STAG" -m "version upgrade to $$STAG"

stagpush:
	git push --tags
