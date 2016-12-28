# The Boot Process

1. Compile the loader script using nasm
```
nasm -f elf32 loader.s
```

2. Link the loader.o with the following command

```
$  ld -T link.ld -melf_i386 loader.o -o kernel.elf

```
3. Obtain copy of the compiled grub from the book's website
    - [Comoiled stage2 grub](http://littleosbook.github.com/files/stage2_eltorito.)
    - GRUB 0.97 by downloading the source from ftp://alpha.gnu.org/gnu/grub/grub-0.97.tar.gz. 
    
4. Build the ISO image using genisoimage

```
    $ mkdir -p iso/boot/grub              # create the folder structure
    $ cp stage2_eltorito iso/boot/grub/   # copy the bootloader
    $ cp kernel.elf iso/boot/             # copy the kernel
```
Create a new grub menu by creating the following config file (menu.lst)

```
    default=0
    timeout=0

    title os
    kernel /boot/kernel.elf
 ```
 Place the file inside the grub folder under iso
 
 ```
  iso
    |-- boot
      |-- grub
      | |-- menu.lst
      | |-- stage2_eltorito
      |-- kernel.elf
 ```
 
 5. Running the Bochs simulator
    - Create a bochs config file
    ``
    megs:            32
    display_library: sdl
    romimage:        file=/usr/share/bochs/BIOS-bochs-latest
    vgaromimage:     file=/usr/share/bochs/VGABIOS-lgpl-latest
    ata0-master:     type=cdrom, path=os.iso, status=inserted
    boot:            cdrom
    log:             bochslog.txt
    clock:           sync=realtime, time0=local
    cpu:             count=1, ips=1000000
    
    ```
    Run the bochs emulator using the following command and check the log file 
    
    ```
     $ bochs -f bochsrc.txt -q
     
     $ cat bochslog.txt
    
    ```
    

     
 
 
