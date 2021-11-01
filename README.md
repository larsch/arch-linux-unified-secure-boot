# Arch Linux Unified Kernel Image Secure Boot

Script to automate installation and update a secure boot enabled EFI image
using [shim](https://github.com/rhboot/shim), [systemd-boot](https://wiki.archlinux.org/title/Systemd-boot)
and a [unified kernel image](https://systemd.io/BOOT_LOADER_SPECIFICATION/#type-2-efi-unified-kernel-images)
signed using a locally generated machine owner key (MOK). This enables secure
boot without modifying system keys (which can brick some devices).

## Secure Boot

The steps for secure boot using this method is:

1. Load `shimx64.efi` (signed by Microsoft)
2. Load `systemd-bootx64.efi` (renamed to `grubx64.efi`) signed with MOK
3. Load unified kernel image, signed with MOK, containing:
   1. Linux Kernel
   2. Initramfs
   3. Kernel command line (arguments)

To secure a system, you must also:

1. [Encrypt the root file system](https://wiki.archlinux.org/title/Dm-crypt), including `/boot`
2. Password-protect the BIOS
3. Enable Secure Boot in BIOS
4. Password protect the MokManager (`mokutil --password`)

## Installation

This assumes that your ESP (EFI System Partition) is mounted at `/efi`, that
kernel (`vmlinuz-linux`) and initramfs (`initramfs-linux.img`) are located in
the default locations in `/boot` and that your primary boot device is `/dev/nvme0n1`.
These can be customized at the top of the main script.

1. Install dependencies: `yay -Syu binutils efibootmgr sbsigntools shim-signed`
1. Copy [`update-unified-kernel-image`](https://github.com/larsch/arch-linux-unified-secure-boot/blob/master/update-unified-kernel-image) to `/usr/bin` and give it executable permission.
1. Copy your kernel boot parameters to `/etc/kernel/cmdline`.
2. Run `update-unified-kernel-image`
3. On the first run, this will:
   1. Generate MOK (`/etc/kernel/keys/MOK.*`) and copy certificate to EFI
      partition (`/EFI/MOK.cer`).
   2. Install systemd-boot. This will create a boot entry "Linux Boot Manager"
      which can be ignored or removed.
   3. Create an EFI boot entry for the Microsoft signed shim called "Shim". This
      must be the default boot entry.
4. Boot into MokManager (should be automatic, since the MOK is not yet enrolled).
5. Enroll the `MOK.cer` certificate.
6. Install [`99-update-unified-kernel-image.hook`](https://github.com/larsch/arch-linux-unified-secure-boot/blob/master/99-update-unified-kernel-image.hook) to `/etc/pacman.d/hooks/` to automatically
   run `update-unified-kernel-image` if Kernel or systemd is updated by pacman.

## Dependencies

* sbsigntools
* binutils (objcopy)
* efibootmgr
* shim-signed (from AUR)

## Authors

 * Jesper Rosenkilde (https://github.com/jbro/)
 * Lars Christensen <larsch@belunktum.dk>
