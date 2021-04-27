# LiveCD for deployment

This repo will produce a LiveCD for image deployment with minimal things that
you need. You will need more development than what is here in this repo,
this is intended to get you started and pointed in the right direction,
not a complete deployment solution.

One of the things that you will need is some way to tell the image what to do.
One suggestion might be a script that looks at `/proc/cmdline` and parses
what it finds there. Another might be something that reaches out to a web
service. At any rate, all that you get here is something that boots :).

Things that you need to make this work:

- RHEL8 host
- lorax installed on that RHEL8 host.

The [documentation for lorax](https://weldr.io/lorax/rhel8-branch/livemedia-creator.html#anaconda-image-install-no-virt)
mentions that you should only use the `--no-virt` option on a machine that you can afford
to lose all data on. I will agree with the author of that document that I've
never had it happen across many runs, but caveat emptor. You *are* running the
system installer on an installed system. It should remain confined to it's chroot,
but we all know that software has no bugs, right?

Note that the pxe-rhel-minimized kickstart will not produce an ISO image. If
you want one of those, you'll need to use rhel-minimized.ks for that.

The command to build these, once you have the kickstarts appropriately modified
to point to local repos is:

`livemedia-creator --project RHEL --releasever 8 --make-pxe-live --ks=pxe-rhel-minimized.ks --no-virt --tmp=$(pwd)`

to make a bootable PXE image and

`livemedia-creator --project RHEL --releasever 8 --make-iso --no-virt --ks=rhel-minimized.ks --tmp=$(pwd)A`

to make a bootable ISO. Of course, replace the location of the tmpdir with
whatever you like.

