# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>
pkgname=nspr
pkgver=4.9.2
pkgrel=1
pkgdesc="Netscape Portable Runtime"
arch=(i686 x86_64)
url="http://www.mozilla.org/projects/nspr/"
license=('MPL' 'GPL')
depends=('glibc')
makedepends=('zip')
options=(!emptydirs)
source=(ftp://ftp.mozilla.org/pub/mozilla.org/nspr/releases/v${pkgver}/src/${pkgname}-${pkgver}.tar.gz
        nspr.pc.in)
md5sums=('1a8cad110e0ae94f538610a00f595b33'
         'bce1611f3117b53fc904cab549c09967')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  if [ "$CARCH" = "x86_64" ]; then
    confflags="--enable-64bit"
  else
    confflags=""
  fi

  sed -e 's/\$(MKSHLIB) \$(OBJS)/\$(MKSHLIB) \$(LDFLAGS) \$(OBJS)/g' \
      -i mozilla/nsprpub/config/rules.mk

  ./mozilla/nsprpub/configure \
      --prefix=/usr \
      --libdir=/usr/lib \
      --includedir=/usr/include/nspr \
      --enable-optimize \
      --disable-debug ${confflags}
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make DESTDIR="${pkgdir}" install

  NSPR_LIBS=`./config/nspr-config --libs`
  NSPR_CFLAGS=`./config/nspr-config --cflags`
  NSPR_VERSION=`./config/nspr-config --version`
  install -m755 -d "${pkgdir}/usr/lib/pkgconfig"
  sed "${srcdir}/nspr.pc.in" -e "s,%libdir%,/usr/lib," \
  	-e "s,%prefix%,/usr," \
	-e "s,%exec_prefix%,/usr/bin," \
	-e "s,%includedir%,/usr/include/nspr," \
	-e "s,%NSPR_VERSION%,${NSPR_VERSION}," \
	-e "s,%FULL_NSPR_LIBS%,${NSPR_LIBS}," \
	-e "s,%FULL_NSPR_CFLAGS%,${NSPR_CFLAGS}," > "${pkgdir}/usr/lib/pkgconfig/nspr.pc"
  chmod 644 "${pkgdir}/usr/lib/pkgconfig/nspr.pc"
  ln -sf nspr.pc "${pkgdir}/usr/lib/pkgconfig/mozilla-nspr.pc"

  chmod 644 ${pkgdir}/usr/lib/*.a

  rm -rf "${pkgdir}/usr/bin/compile-et.pl" \
	"${pkgdir}/usr/bin/prerr.properties" \
	"${pkgdir}/usr/share/aclocal/nspr.m4" \
	"${pkgdir}/usr/include/nspr/md"
}
