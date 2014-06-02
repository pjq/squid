squid
=====

Squid for mips

###./configure
Before you need config the cross compiler tools to the PATH, something like this:
```
export PATH=/opt/buildroot-gcc342/bin/:${PATH}
```
Add to the ~/.bashrc
```
./configure --host=mipsel-linux --build=i686-ubuntu-linux  --cache-file=mips-cache --disable-epoll
```
Here you'll meet some issues, please refer to http://blog.csdn.net/kaptek/article/details/8935773

After configure, I have changed the Makefile of squid:

To avoid 
```
./cf_gen: ./cf_gen: cannot execute binary file
```
First you should compile the source code with the native compiler, not the cross compiler, and the copy to the project root directory.

Then edit ./src/Makefile
```
cf_parser.h: cf.data cf_gen$(EXEEXT)
     ../cf_gen cf.data $(srcdir)/cf.data.depend 
```
I have change it from ./cf_gen to ../cf_gen


Second add the romfs target to ./Makefile, copy the squid to the romfs/bin/, because this is just test, so for more further usage, need consider all the specific files needed.
```
romfs:
	$(ROMFSINST) src/squid /bin/squid 
```
