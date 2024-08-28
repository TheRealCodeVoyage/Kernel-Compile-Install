# Kernel-Compile-Install

Steps on how to compile and install linux kernel by yourself

## Prequisite

### Step 1: Install Dependencies

```bash
sudo apt install bc binutils bison dwarves flex gcc git gnupg2 gzip libelf-dev libncurses5-dev libssl-dev make openssl pahole perl-base rsync tar xz-utils
```

### Step 2: download the Kernel and its PGP Verification Signature

**[Kernel.org](https://www.kernel.org/)**

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.10.6.tar.xz
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.10.6.tar.sign
```

## Compile

### Step 1: decompress .xz file

```bash
unxz --keep linux-*.tar.xz
```

### Step 2: Get public encryption key(s) and verify signed  file

**gpg2: OpenPGP encryption and signing tool**

```bash
gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org

gpg2 --verify linux-*.tar.sign
```

### Step 3: Extract tar archive file

```bash
tar -xf linux-*.tar
```
### Step 4: create .config file. Copy the current .config file from the OS

```bash
cp /boot/config-"$(uname -r)" .config
file .config
```

### Step 5: Updating the configuration

**To update an existing `.config` file, the `make` command is used with the target `olddefconfig`. Broken down, this is `old` `def`ault `config`uration. This will take the "old configuration file" (which is currently saved as .config as a literal copy of your distribution's configuration) and check for any new configuration options that were added to the Linux codebase since**

```bash
make olddefconfig
```

**The most reliable way to modify your config file is via the `menuconfig` command**

```bash
make menuconfig
```

**General Setup -> CONFIG_LOCALVERSION -> "-CodeVoyageWithIman"**

### Step 6: Compile and time the compile time

```bash
make -j$(nproc) 2>&1 | tee log
```

**If you want, you can time your compile process, in that case use the commnad below:**

```bash
(time make -j$(nproc) 2>&1 | tee log) 2>&1 | tee build_time.log
```

## Install

### Will install all the compile kernel file

```bash
sudo make modules_install -j$(nproc)
sudo make headers_install -j$(nproc)
sudo make install -j$(nproc)
```

## After reboot

### check your kernel name

```bash
uname -r
```
