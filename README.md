# Modern n64sdk

This repo describes how to get the [n64
sdk](http://ultra64.ca/files/software/other/sdks/n64sdk.7z) compiling on a modern OS (Linux,
Windows, macOS).

## Steps
### Download the [sdk](http://ultra64.ca/files/software/other/sdks/n64sdk.7z).

Download the [sdk](http://ultra64.ca/files/software/other/sdks/n64sdk.7z), and extract to a folder. We'll call this folder `N64_SDK`.

### Compile the GCC toolchain

Clone the n64chain repo found here: https://github.com/tj90241/n64chain.

Copy the `tools` directory into another directory. We'll call this folder
`N64_TOOLCHAIN`.

Run either the `build-linux64-toolchain.sh` script for linux or mac. Run the
`build-windows64-toolchain.sh` for windows. This'll take a while.

After running this command, make sure that `N64_TOOLCHAIN` is on your path:

```bash
export PATH="$N64_TOOLCHAIN/bin:$PATH"
```

### Set up additional tools

Download all of the latest versions of these tools, extract them, and place
them somewhere on your path (`$N64_TOOLCHAIN/bin` for example):

- https://github.com/trhodeos/spicy/releases
- https://github.com/trhodeos/makemask/releases

### Apply patches to n64sdk

The first file you'll need to modify is `$N64_SDK/ultra/usr/include/make/PRdefs`.

Make the following modifications:

- Change `CC = gcc` to `CC = mips64-elf-gcc`
- Change `LD = ld` to `LD = mips64-elf-gcc`
- Change `MAKEROM = mild` to `MAKEROM = spicy`
- Change `GCCFLAG` to `GCCFLAG = -c -I$(INC) -D_MIPS_SZLONG=32 -D_MIPS_SZINT=32 -D_LANGUAGE_C -D_ULTRA64 -D__EXTENSIONS__ -mabi=32 -march=vr4300 -mtune=vr4300`

For any demo you'd like to build, modify the `Makefile` such that:

- `FINAL = YES` is not commented out
- Comment out the `OPTIMIZER` line (spicy doesn't play nicely with optimizer params for now).

## Export the appropriate variables

Note that these will need to be set for every terminal you use to build.

```bash
export ROOT=$N64_SDK # defined above
export GCCDIR=$ROOT/GCC
export PATH=$N64_TOOLCHAIN/bin:$PATH
```