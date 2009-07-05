
minimaldepends:
	tar -jx --file dotnet-protobufs*.bz2 && \
	tar -jx --file FreeImage-*.bz2 && \
	tar -jx --file curl*.bz2 && \
	tar -jx --file boost*.bz2 && \
	tar -jx --file ois*.bz2 && \
	unzip -o SDL-1.3*.zip && \
	tar -jx --file ogre*.bz2 && \
	tar -jx --file antlr-*.bz2 && \
	tar -jx --file bullet-*.bz2 && \
	tar -jx --file awesomium*.bz2 && \
	( [ -L lib/libboost_iostream-mt-d-1_35.a ] && rm -f lib/libboost_iostream-mt-d-1_35.a || true ) && \
	ranlib installed-bullet/lib/*.a && \
	ranlib lib/*.a
depends: minimaldepends

fulldepends: depends

