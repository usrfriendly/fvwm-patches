# FVWM Patches

I have no idea where these originated, or their license, nor do I have the skills to maintain them, or even know *anything* about them.  If you've got this info, file an issue or a PR to add to this README, and I'll update.

I pilfered the patches from [willscreel](https://github.com/willscreel)

I'm not a Debian packager, FVWM contributor or anything.  These instructions are offered with no warranty, use at your own risk, blah blah.  It's mostly because I've just done it and want to share.

#### I want to document the build process to be as distro-agnostic as possible, but still in a package manager.

For Debian-based distros, you'll need to do the following, bastardized from [here](https://wiki.debian.org/BuildingTutorial)

```# apt install build-essential fakeroot devscripts```

Run the following in a directory of your choice

`$ apt source fvwm --download-only` (to download the source, but not patch it yet)

Extract the source archive, which will be fvwm_$VERSION_.orig.tar.gz, as of right now 2.6.8

Extract the Debian directory, which will be fvwm_$VERSION.debian.tar.xz

    #mv debian fvwm-$VERSION/

Copy the patches into the debian/patches folder, and CD into the source directory

    cp fvwm-patches/[0-1][0-9]-*.patch fvwm-$VERSION/debian/patches
    cd fvwm-$VERSION

This package uses `quilt` to handle patching.  We didn't patch earlier to patch now, but must first add the new patch files to the patch order, and apply the patches.

    export QUILT_PATCHES debian/patches
    quilt import [0-1][0-9]-*.patch
    quilt push -a

If you were so inclined, you can remove the patches with `quilt pop -a`, or check which patches were applied with `quilt applied`

Now we're ready to install build dependencies and build the package:

    sudo apt build-deps fvwm
    dpkg-buildpackage -us -uc

The packages will be built a directory above where you run this command.  Install FVWM first to sort out the identical dependencies, then this built package with `dpkg -i fvwm_2.6.8-1_amd64.deb`.  Just make sure the version/arch match yours if it's different.

Good luck configuring it, I'll document what I can find about the patches in this repo.
