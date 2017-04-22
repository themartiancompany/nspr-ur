# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.14
pkgrel=1
pkgdesc="Netscape Portable Runtime"
url="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR"
arch=(i686 x86_64)
license=(MPL GPL)
depends=(glibc sh)
makedepends=(zip)
source=(https://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/${pkgname}-${pkgver}.tar.gz)
sha1sums=('4c9ffda940882229104090f8fc8539f6412e1ff7')
sha256sums=('64fc18826257403a9132240aa3c45193d577a84b08e96f7e7770a97c074d17d5')

build() {
  cd $pkgname-$pkgver
  ./nspr/configure \
      --prefix=/usr \
      --libdir=/usr/lib \
      --includedir=/usr/include/nspr \
      --enable-optimize \
      --disable-debug \
      $([[ $CARCH == x86_64 ]] && echo --enable-64bit)
  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
  ln -s nspr.pc "$pkgdir/usr/lib/pkgconfig/mozilla-nspr.pc"
  rm -r "$pkgdir"/usr/bin/{compile-et.pl,prerr.properties} \
        "$pkgdir"/usr/include/nspr/md
}
