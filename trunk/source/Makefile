
##### Levels of installation

# Default rule:
depends: minimaldepends installed-mono

minimaldepends: nonroot-dependencies compiled-dependencies 

fulldepends: root-dependencies compiled-dependencies 


##### Rules to install each package
installed-%:
	./install $(INSTALLFLAGS) $(patsubst installed-%,%,$@)

installed-ois: installed-ogre

installed-ogre: installed-freeimage installed-boost


##### 

compiled-dependencies: installed-ois installed-ogre installed-freeimage installed-antlr installed-bullet

optional-dependencies: installed-mono

nonroot-dependencies: installed-curl installed-boost 

root-dependencies:
	sudo ./install --use-root --user `whoami` boost curl


