# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.13
pkgrel=1
pkgdesc="Netscape Portable Runtime"
arch=(i686 x86_64)
url="http://www.mozilla.org/projects/nspr/"
license=('MPL' 'GPL')
depends=('glibc')
makedepends=('zip')
options=('!emptydirs')
source=(https://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/${pkgname}-${pkgver}.tar.gz)
sha1sums=('c9274bb5cc33a388a7e8cd9b378873c59190f74b')
sha256sums=('19c33334bb3fa6d24800ffa65d7d806c54ad5f8c3758a5c11352ad43212ab181')

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
