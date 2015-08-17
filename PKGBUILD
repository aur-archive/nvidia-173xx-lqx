# Maintainer: none(orphaned)

pkgname=nvidia-173xx-lqx
pkgver=173.14.31
_kernver='3.0.0-lqx'
pkgrel=1
pkgdesc="NVIDIA drivers for linux-lqx, 173xx branch."
arch=('i686' 'x86_64')
[ "$CARCH" = "i686"   ] && ARCH=x86
[ "$CARCH" = "x86_64" ] && ARCH=x86_64
url="http://www.nvidia.com/"
depends=('linux>=3.0' 'linux<3.1' 'nvidia-173xx-utils')
makedepends=('linux-headers>=3.0' 'linux-headers<3.1')
conflicts=('nvidia-96xx' 'nvidia')
license=('custom')
install=nvidia.install
source=("http://download.nvidia.com/XFree86/Linux-$ARCH/${pkgver}/NVIDIA-Linux-$ARCH-${pkgver}-pkg0.run")
md5sums=('01cd3b91196cd4495f56c34494f310b5')
[ "$CARCH" = "x86_64" ] && md5sums=('d53e8faf8ac90ad98b8998f25f121535')

build() {
	cd $srcdir
	sh NVIDIA-Linux-$ARCH-${pkgver}-pkg0.run --extract-only
	cd NVIDIA-Linux-$ARCH-${pkgver}-pkg0
	cd usr/src/nv/
	ln -s Makefile.kbuild Makefile
	make SYSSRC=/lib/modules/${_kernver}/build module || return 1
}

package() {
	cd $srcdir/NVIDIA-Linux-$ARCH-${pkgver}-pkg0/usr/src/nv/
	mkdir -p $pkgdir/lib/modules/${_kernver}/kernel/drivers/video/
	install -m644 nvidia.ko $pkgdir/lib/modules/${_kernver}/kernel/drivers/video/
        mkdir -p $pkgdir/etc/modprobe.d
        echo "blacklist nouveau" >> $pkgdir/etc/modprobe.d/nouveau_blacklist.conf || return 1
	sed -i -e "s/KERNEL_VERSION='.*'/KERNEL_VERSION='${_kernver}'/" $startdir/nvidia.install
        # gzip -9 module to save some space
        gzip "${pkgdir}/lib/modules/${_kernver}/kernel/drivers/video/nvidia.ko"
}
