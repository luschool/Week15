# Week 15 Chapter 16 Compile documentation
### By Luschool

This is my documentation for the assignment to compile something from source. 
Since our group project uses iPXE and it requires us to build it anyways this
allows me to learn for the assignment and apply that learning for our group
project. 


**Links -**
* [iPXE](http://ipxe.org/)
* [iPXE source files](http://git.ipxe.org/ipxe.git)


## Summary -  

I started by downloading the source files from github -` git clone git://git.ipxe.org/ipxe.git`
 
Now I must navigate to the /ipxe/src/ directory to make any modifications that may need to be done.

Some files may need to be accessed from HTTPS so we need to enable that in the source files.
To do that we must edit - `config/general.h` and change -
`#undef	DOWNLOAD_PROTO_HTTPS` to `#define	DOWNLOAD_PROTO_HTTPS`

To stop an infinite loop from hapening on some computers we need to insert a script to direct it
to our menu. We accomplish that by navigating to ipxe/src/ and running mkfile `chainb.ipxe`

edit with `nano chainb.ipxe` to add the following lines - 
```
#!ipxe
  
dhcp
chain --autofree tftp://192.168.1.30/pboot2.ipxe
```
This is an ipxe script that will direct our bootloader to pavels custom menu located on our
raspberry pi TFTP server. Without this script directing us to the menu it would cause 
an infinite loop as described on iPXEs website -

```
When the chainloaded iPXE starts up, it will issue a fresh DHCP request and boot whatever the DHCP server hands out. 
The DHCP server is currently set up to hand out the iPXE image, which means that you will be stuck in an infinite loop: 
PXE will load iPXE which will load iPXE which will load iPXE which will load iPXE…
```

Now the source files need to be compiled and we must include the chainb.ipxe script by running the
following command - `make bin/undionly.kpxe EMBED=chainb.ipxe`
After the compilation completes it spit out the needed `undionly.kpxe` file in the bin/ directory. 


