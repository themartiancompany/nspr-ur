# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.22
pkgrel=1
pkgdesc="Netscape Portable Runtime"
url="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR"
arch=(x86_64)
license=(MPL GPL)
depends=(glibc sh)
makedepends=(zip)
source=(https://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/nspr-${pkgver}.tar.gz)
sha1sums=('69bd044d4a258a9c1cb1062a182fb8a30f0ad641')
sha256sums=('c9e4b6cc24856ec93202fe13704b38b38ba219f0f2aeac93090ce2b6c696d430')

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
