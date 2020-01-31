# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.25
pkgrel=1
pkgdesc="Netscape Portable Runtime"
url="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR"
arch=(x86_64)
license=(MPL GPL)
depends=(glibc sh)
makedepends=(zip)
source=(https://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/nspr-${pkgver}.tar.gz)
sha1sums=('fe02a9056ce867677401b3d6372cdb62f7c7aad4')
sha256sums=('0bc309be21f91da4474c56df90415101c7f0c7c7cab2943cd943cd7896985256')

prepare() {
  cd nspr-$pkgver/nspr
}

build() {
  cd nspr-$pkgver/nspr
  ./configure \
      --prefix=/usr \
      --libdir=/usr/lib \
      --includedir=/usr/include/nspr \
      --enable-optimize \
      --disable-debug \
      $([[ $CARCH == x86_64 ]] && echo --enable-64bit)
  make ${SOURCE_DATE_EPOCH:+
    SH_NOW="${SOURCE_DATE_EPOCH}000000"
    SH_DATE="$(date --utc --date="@$SOURCE_DATE_EPOCH" '+%Y-%m-%d %T')"
  }
}

package() {
  cd nspr-$pkgver/nspr
  make DESTDIR="$pkgdir" install
  ln -s nspr.pc "$pkgdir/usr/lib/pkgconfig/mozilla-nspr.pc"
  rm -r "$pkgdir"/usr/bin/{compile-et.pl,prerr.properties} \
        "$pkgdir"/usr/include/nspr/md
}
