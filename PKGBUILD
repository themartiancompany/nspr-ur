# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.10.2
pkgrel=1
pkgdesc="Netscape Portable Runtime"
arch=(i686 x86_64)
url="http://www.mozilla.org/projects/nspr/"
license=('MPL' 'GPL')
depends=('glibc')
makedepends=('zip')
options=('!emptydirs')
source=(ftp://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/${pkgname}-${pkgver}.tar.gz)
sha1sums=('650e4aa35d58624bc1083ed585c81c4af09cf23c')
md5sums=('f0d254da0b2b870d9a5fa094e879d4b8')

build() {
  cd $pkgname-$pkgver

  if [ "$CARCH" = "x86_64" ]; then
    _confflags="--enable-64bit"
  else
    _confflags=""
  fi

  ./nspr/configure \
      --prefix=/usr \
      --libdir=/usr/lib \
      --includedir=/usr/include/nspr \
      --enable-optimize \
      --disable-debug ${_confflags}
  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  ln -s nspr.pc "$pkgdir/usr/lib/pkgconfig/mozilla-nspr.pc"
  rm -r "$pkgdir"/usr/bin/{compile-et.pl,prerr.properties} \
         "$pkgdir/usr/include/nspr/md"
}
