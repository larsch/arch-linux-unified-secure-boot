# Maintainer: Lars Christensen <larsch@belunktum.dk>
pkgname=unified-secure-boot
pkgver=0.2
pkgrel=1
epoch=
pkgdesc="Scripts to automate generation of unified kernel image for secure boot"
arch=('x86_64')
url="https://github.com/larsch/arch-linux-unified-secure-boot"
license=('MIT')
groups=()
depends=(sbsigntools binutils efibootmgr shim-signed)
makedepends=()
checkdepends=()
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=("99-update-unified-kernel-image.hook"
        "update-unified-kernel-image")
noextract=()
sha256sums=('560dcaa2ecf09d201d85eaee281c2c6e9f6bd87c73c1b81a7e4cf9eabcc3715e'
            '86e1bef101eae636bb12205d4242dc3c599f582f992c775bf666be85312e0150')
validpgpkeys=()

package() {
	install -D -m 0755 -o root -g root "$srcdir/update-unified-kernel-image" "$pkgdir/usr/bin/update-unified-kernel-image"
	install -D -m 0644 -o root -g root "$srcdir/99-update-unified-kernel-image.hook" "$pkgdir/etc/pacman.d/hooks/99-update-unified-kernel-image.hook"
}
