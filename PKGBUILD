# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.27
pkgrel=1
pkgdesc="Netscape Portable Runtime"
url="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR"
arch=(x86_64)
license=(MPL GPL)
depends=(glibc sh)
makedepends=(zip)
source=(https://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/nspr-${pkgver}.tar.gz)
sha1sums=('7a318da847beb5aff04446fc2db11b253b6fb7c8')
sha256sums=('6d495192b6ab00a3c28db053492cf794329f7c0351a5728db198111a1816e89b')

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
      --enable-64bit
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
