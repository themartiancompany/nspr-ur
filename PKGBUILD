# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.10.6
pkgrel=1
pkgdesc="Netscape Portable Runtime"
arch=(i686 x86_64)
url="http://www.mozilla.org/projects/nspr/"
license=('MPL' 'GPL')
depends=('glibc')
makedepends=('zip')
options=('!emptydirs')
source=(ftp://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/${pkgname}-${pkgver}.tar.gz)
md5sums=('6ab81e8d508457905223eaf4ed0a973b')
sha1sums=('9f3f278f7f31574b2cdbb99d9703c58e51cd3e1c')

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
