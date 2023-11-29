
# BUILD YOUR OPERRATING SYSTEM USING BUSYBOX 

To start your working process first we have download some utility 


## Setting up the enviroment for devolopment

We are goin to install "flex" for generating lexical analyzers,also we need to install "bision" for generating parsers.Then we need to install "libssl-dev" for development package for OpenSSL, which includes the necessary header files and also we will need another tool for build and process 'libelf" header files. 
You can download all of this tool using following commands.This commands will work on Debian-based system.
```bash
  sudo apt-get install flex
  sudo apt-get install bison
  sudo apt-get install libelf-dev
  sudo apt-get install libssl-dev
```
    
## Now start building the project
We have to create two shell sceript file one for busybox main another for building. I will name them as "busybox.sh" and "temp.sh" you can name them as you wanted. Make sure both of this file are in same directory. Now change this file two permission using "chmod 777 busybox.sh" and "chmod 777 temp.sh". Make sure you are on this directory on your terminal.


### Now run this code sniptes in "temp.sh" file to run file use "./temp.sh" on your terminal make sure you are on this folder. 

```bash 
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# KERNEL
KERNEL_MAJOR=$(echo $KERNEL_VERSION | sed 's/\([0-9]*\)[^0-9].*/\1/') 
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v$KERNEL_MAJOR.x/linux-$KERNEL_VERSION.tar.xz
tar -xf linux-$KERNEL_VERSION.tar.xz
cd linux-$KERNEL_VERSION
    make defconfig
    make -j8 || exit
cd ..
```
It will install all file for kernel.

### Then remove all of this code from this and save them in "busybox.sh" file. and run following code and it will build busybox.

```bash 
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# BUSYBOX
wget https://busybox.net/downloads/busybox-$BUSYBOX_VERSION.tar.bz2
tar -xf busybox-$BUSYBOX_VERSION.tar.bz2
cd busybox-$BUSYBOX_VERSION
make defconfig
make  # Add this line to actually build busybox
cd ..

cd ..

```

## Now again remove all the code from temp.sh file and add them in busybox.sh file your busybox file will look something like this.

```bash 
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# KERNEL
KERNEL_MAJOR=$(echo $KERNEL_VERSION | sed 's/\([0-9]*\)[^0-9].*/\1/') 
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v$KERNEL_MAJOR.x/linux-$KERNEL_VERSION.tar.xz
tar -xf linux-$KERNEL_VERSION.tar.xz
cd linux-$KERNEL_VERSION
    make defconfig && make -j8 || exit
cd ..

# BUSYBOX
wget https://busybox.net/downloads/busybox-$BUSYBOX_VERSION.tar.bz2
tar -xf busybox-$BUSYBOX_VERSION.tar.bz2
cd busybox-$BUSYBOX_VERSION
    make defconfig && make
cd ..

cd ..
```
After running this two file you will be able download all of nessary dependency for your kernel keep in mind it will take time based on your internet connection and your mechine configaration.

## Now open the terminal change directory to src/busybox-1.35.0 using command.
```bash
cd src/busybox-1.35.0
```

After going into this folder we will set our build config to yes using "vim" if you don't have vim you can simply install it by using this command .
```bash
sudo apt-get update
sudo apt-get install vim
```
Now in your terminal use this command
```bash
vim .config
```
it will open config menu and set "#Build Options" "CONFIG_STATIC=y" 
![Here is the ss for clear understanding](https://drive.google.com/file/d/1kb0cLNbf2Ww80CmUkPo2dpl8jhqwnkva/view?usp=sharing)
And quit the terminal using this command.

```bash
:qa!
```

## Change directory to your main folder and run this code in your temp.sh file.
```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# BUSYBOX
cd busybox-$BUSYBOX_VERSION
make defconfig && sed 's/^.*CONFIG_STATIC.*$/CONFIG_STATIC=y/g' -i .config && make -j8 || exit
make  
cd ..

cd ..
```
## After this remove the code and your busybox.sh file will look like this.

```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# KERNEL
KERNEL_MAJOR=$(echo $KERNEL_VERSION | sed 's/\([0-9]*\)[^0-9].*/\1/') 
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v$KERNEL_MAJOR.x/linux-$KERNEL_VERSION.tar.xz
tar -xf linux-$KERNEL_VERSION.tar.xz
cd linux-$KERNEL_VERSION
make defconfig && make -j8 || exit
cd ..

# BUSYBOX
wget https://busybox.net/downloads/busybox-$BUSYBOX_VERSION.tar.bz2
tar -xf busybox-$BUSYBOX_VERSION.tar.bz2
cd busybox-$BUSYBOX_VERSION
sed 's/^.*CONFIG_STATIC[^_].*$/CONFIG_STATIC=y/g' -i .config && make -j8 || exit
make      
cd ..

cd ..

```
## Now open your terminal and use this command to recursivley copying the path.
```bash
cp -r src/linux-5.15.7/arch/x x86/
cp -r src/linux-5.15.7/arch/x86_64 x86_64/
cp -r src/linux-5.15.7/arch/xtensa xtensa/
```
## After this run this command
```bash
cp src/linux-5.15.7/arch/x86_64/boot/bzImage ./
```

### now update busybox.sh file
```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# KERNEL
KERNEL_MAJOR=$(echo $KERNEL_VERSION | sed 's/\([0-9]*\)[^0-9].*/\1/') 
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v$KERNEL_MAJOR.x/linux-$KERNEL_VERSION.tar.xz
tar -xf linux-$KERNEL_VERSION.tar.xz
cd linux-$KERNEL_VERSION
make defconfig && make -j8 || exit
cd ..

# BUSYBOX
wget https://busybox.net/downloads/busybox-$BUSYBOX_VERSION.tar.bz2
tar -xf busybox-$BUSYBOX_VERSION.tar.bz2
cd busybox-$BUSYBOX_VERSION
sed 's/^.*CONFIG_STATIC[^_].*$/CONFIG_STATIC=y/g' -i .config && make -j8 || exit
make      
cd ..

cd ..
cp src/linux-$KERNEL_VERSION/arch/x86_64/boot/bzImage ./

```

## Now agian back on temp.sh file and run this following code
```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0
BUSYBOX_DIR="../../src/busybox-$BUSYBOX_VERSION"

# initrd
mkdir -p initrd/bin initrd/dev initrd/proc initrd/sys
cd initrd/bin

# Check if busybox binary exists
if [ -e "$BUSYBOX_DIR/busybox" ]; then
    cp "$BUSYBOX_DIR/busybox" ./
    
    # Create symbolic links for all busybox applets
    ./busybox --list | xargs -I {} ln -s busybox {}
else
    echo "Error: BusyBox binary not found in $BUSYBOX_DIR"
    exit 1
fi

cd ../../

```

In here we are telleing that to Create a initrd directory and clone all the busybox content.

## We are almost done building the project now we have to tell your kernel what to do before boot up agin in temp.sh file run this following code. 
```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

# initrd
mkdir initrd
cd initrd

echo '#!/bin/bash' > init
echo 'mount -t sysfs sysfs /sys' >> init
echo 'mount -t proc proc /proc' >> init
echo 'mount -t devtmpfs devtmpfs /dev' >> init
echo 'sysctl -w kernel.printk="2 4 1 7"' >> init
echo '/bin/sh' >> init
echo 'poweroff -f' >> init

chmod +x init
find . | cpio -o -H newc > ../initrd.img
cd ..
```
now update your busybox.sh file

```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0

mkdir -p src
cd src

# KERNEL
KERNEL_MAJOR=$(echo $KERNEL_VERSION | sed 's/\([0-9]*\)[^0-9].*/\1/') 
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v$KERNEL_MAJOR.x/linux-$KERNEL_VERSION.tar.xz
tar -xf linux-$KERNEL_VERSION.tar.xz
cd linux-$KERNEL_VERSION
make defconfig && make -j8 || exit
cd ..

# BUSYBOX
wget https://busybox.net/downloads/busybox-$BUSYBOX_VERSION.tar.bz2
tar -xf busybox-$BUSYBOX_VERSION.tar.bz2
cd busybox-$BUSYBOX_VERSION
sed 's/^.*CONFIG_STATIC[^_].*$/CONFIG_STATIC=y/g' -i .config && make -j8 || exit
make      
cd ..

cd ..

# Copy the kernel image
cp src/linux-$KERNEL_VERSION/arch/x86_64/boot/bzImage ./

# initrd
mkdir initrd
cd initrd
mkdir -p bin dev proc sys
cd bin
cp ../../src/busybox-$BUSYBOX_VERSION/busybox ./
for prog in $(./busybox --list); do
   ln -s /bin/busybox ./$prog
done   

cd ..

# Create init script
echo '#!/bin/bash' > init
echo 'mount -t sysfs sysfs /sys' >> init
echo 'mount -t proc proc /proc' >> init
echo 'mount -t devtmpfs udev /dev' >> init
echo 'sysctl -w kernel.printk="2 4 1 7"' >> init
echo '/bin/sh' >> init
echo 'poweroff -f' >> init

chmod -R 777 .
find . | cpio -o -H newc > ../initrd.img
cd ..

cd ..

```
## Now its time to boot our KERNEL for this run our temp.sh file with this script

```bash
#!/bin/bash

KERNEL_VERSION=5.15.7
BUSYBOX_VERSION=1.35.0
qemu-system-x86_64 -kernel bzImage -initrd initrd.img
```

## If encounter any further erro make sure to install Qemu in your system by using this simple command.
```bash
sudo apt-get install qemu-system-x86
```

## wala you have build your first operating system.
![Boot Menu](https://drive.google.com/file/d/1bn2PWBhrzsav1X1BKVq-ofg3TI6cXCAx/view?usp=sharing)