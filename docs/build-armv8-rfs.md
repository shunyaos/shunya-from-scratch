# Steps

## Step 1: Install User dependencies:

```
$ sudo apt-get update
$ sudo apt install gparted git build-essential libncurses5 wget u-boot-tools zlib1g-dev ncurses-dev cmake build-essential checkinstall cmake pkg-config lzop libc6 libstdc++6 debootstrap qemu-user-static binfmt-support unzip
```

## Step 2: Fetch the root filesystem and extract it

**Note: Replace xxxx with console or desktop depending on the image you want to build on top of**

```
cd ~/
mkdir -p workspace/shunya-from-scratch
cd workspace/shunya-from-scratch
wget http://releases.shunyaos.org/Generic-Shunya-RFS-Armv8/shunya-xxxx-rfs-armv8-0.1.zip
unzip shunya-xxxx-rfs-armv8-0.1.zip
```

## Step 3: Make a folder named rootfs in the same directory

**Note: Replace xxxx with console or desktop depending on the image you downloaded**

```
cd shunya-xxxx-rfs-armv8-0.1 && mkdir -p rootfs
```

## Step 4: Resize the filesystem(optional)

**Note: Replace xxxx with console or desktop depending on the image you downloaded**

```
e2fsck -f shunya-xxxx-rfs-armv8.ext4
sudo resize2fs shunya-xxxx-rfs-armv8.ext4 2G
```

**You can change the 2G with the size you want**

## Step 5: Mount the file-system on the rootfs folder

**Note: Replace xxxx with console or desktop depending on the image you downloaded**

```
sudo mount -o loop shunya-xxxx-rfs-armv8.ext4 rootfs/
sudo cp -rf /usr/bin/qemu-aarch64-static rootfs/usr/bin
sudo rm -rf rootfs/etc/resolv.conf
echo "nameserver 127.0.1.1" | sudo tee rootfs/etc/resolv.conf
```

## Step 6: Now chroot into the rfs:

```
sudo chroot rootfs
```

**You have successfully chrooted into the RFS**

## Step 7: Update the system and install necessary packages.

```
bash-4.4# opkg update
```

**Install the packages of your choice. Example: nano**

```
bash-4.4# opkg install nano
```

## Step 8: Add hostname to /etc/hostname

```
bash-4.4# echo ‘device_name’ > /etc/hostname     
```

**Example: echo hikey960 >/etc/hostname**

### Step 9: Add a new user(optional)

```
bash-4.4# adduser username
```

**This will tell you to enter the password and re-enter the password. Fill the necessary info**

**Add the user to the sudo group**

```
bash-4.4# addgroup username sudo
```

**Congrats! Your customized Shunyaos root-filesystem is ready**
