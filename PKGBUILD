# Maintainer: Lars Christensen <larsch@belunktum.dk>
pkgname=unified-secure-boot
pkgver=0.3
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
sha256sums=('2ed40846054ede6f7948db1cdab13675423649f0b240712d3a7b5b8492a22994'
            'ec210f02b7ebc93b2c06d1e35b61cf3d5ba3a3922bf830209e7984d935bda144')
validpgpkeys=()

package() {
	install -D -m 0755 -o root -g root "$srcdir/update-unified-kernel-image" "$pkgdir/usr/bin/update-unified-kernel-image"
	install -D -m 0644 -o root -g root "$srcdir/99-update-unified-kernel-image.hook" "$pkgdir/etc/pacman.d/hooks/99-update-unified-kernel-image.hook"
}
