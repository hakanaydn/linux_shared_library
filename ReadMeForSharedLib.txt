hakan@hakan:~/Desktop/SharedLibraryExample/Library$ gcc -c -Wall -Werror -fpic mathfunc.c
hakan@hakan:~/Desktop/SharedLibraryExample/Library$ gcc -shared -o libmathfunc.so mathfunc.o
hakan@hakan:~/Desktop/SharedLibraryExample/Library$ ls
libmathfunc.so  mathfunc.c  mathfunc.h  mathfunc.o
hakan@hakan:/usr/local/lib$ sudo mkdir sharedFolder
hakan@hakan:/usr/local/lib$ sudo cp ~/Desktop/SharedLibraryExample/Library/libmathfunc.so /usr/local/lib/sharedFolder/
hakan@hakan:/usr/local/lib/sharedFolder$ cd /etc/ld.so.conf.d/
hakan@hakan:/etc/ld.so.conf.d$ sudo cp libc.conf libShared.conf
hakan@hakan:/etc/ld.so.conf.d$ sudo nano libShared.conf 
hakan@hakan:/etc/ld.so.conf.d$ cat libShared.conf 
                                # libc default configuration
                                /usr/local/lib/sharedFolder
hakan@hakan:/etc/ld.so.conf.d$ sudo ldconfig -p| grep libmathfunc 
hakan@hakan:/etc/ld.so.conf.d$ sudo ldconfig 
hakan@hakan:/etc/ld.so.conf.d$ sudo ldconfig -p| grep libmathfunc 
	libmathfunc.so (libc6,x86-64) => /usr/local/lib/sharedFolder/libmathfunc.so
hakan@hakan:/usr/local/include$ sudo mkdir sharedInc
hakan@hakan:/usr/local/include/sharedInc$ sudo cp ~/Desktop/SharedLibraryExample/Library/mathfunc.h /usr/local/include/sharedInc/
hakan@hakan:~/Desktop/SharedLibraryExample$ gcc -Wall -I /usr/local/include/sharedInc/ -L /usr/local/lib/sharedFolder/ main.c -lmathfunc -o output 
hakan@hakan:~/Desktop/SharedLibraryExample$ ./output 
                                          TOPLA : 15
                                          CIKAR : 5
hakan@hakan:~/Desktop/SharedLibraryExample$ ldd output | grep libmath
	libmathfunc.so => /usr/local/lib/sharedFolder/libmathfunc.so (0x00007f6b02c46000)
