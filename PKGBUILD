# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

pkgname=nspr
pkgver=4.33
pkgrel=1
pkgdesc="Netscape Portable Runtime"
url="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR"
arch=(x86_64)
license=(MPL GPL)
depends=(glibc sh)
makedepends=(zip mercurial)
_revision=5f753966dc01e1872eb4fee6e7b6d0a4fd3daad2
source=("hg+https://hg.mozilla.org/projects/nspr#revision=$_revision")
sha256sums=('SKIP')

pkgver() {
  cd nspr
  hg id -t | sed 's/^NSPR_//;s/_RTM$//;s/_/./g'
}

prepare() {
  cd nspr
}

build() {
  cd nspr
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
  cd nspr
  make DESTDIR="$pkgdir" install
  ln -s nspr.pc "$pkgdir/usr/lib/pkgconfig/mozilla-nspr.pc"

  rm -r "$pkgdir"/usr/include/nspr/md
  rm "$pkgdir"/usr/bin/{compile-et.pl,prerr.properties}
}

# vim:set sw=2 et:
