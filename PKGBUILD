# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.10.5
pkgrel=1
pkgdesc="Netscape Portable Runtime"
arch=(i686 x86_64)
url="http://www.mozilla.org/projects/nspr/"
license=('MPL' 'GPL')
depends=('glibc')
makedepends=('zip')
options=('!emptydirs')
source=(ftp://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/${pkgname}-${pkgver}.tar.gz)
md5sums=('c903557c620b4b84457f57ddc80934c4')
sha1sums=('891de2b62ad42f9b38808a0aa58c95e8cbd5c9a9')

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
