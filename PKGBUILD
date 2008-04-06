# $Id: PKGBUILD,v 1.14 2008/03/12 21:47:52 jgc Exp $
# Maintainer: Alexander Baldeck <alexander@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
pkgname=nspr
pkgver=4.7
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
md5sums=('f937c37f45b116130fef34b15afb6fac'
         'bce1611f3117b53fc904cab549c09967')

build() {
  cd ${startdir}/src/${pkgname}-${pkgver}
  [ "$CARCH" = "x86_64" ] && confflags="--enable-64bit"
  ./mozilla/nsprpub/configure \
  	--prefix=/usr \
	--libdir=/usr/lib \
	--includedir=/usr/include/nspr \
	--enable-optimize="${CFLAGS}" \
	--disable-debug ${confflags} || return 1
  make || return 1
  make DESTDIR=${startdir}/pkg install || return 1

  NSPR_LIBS=`./config/nspr-config --libs`
  NSPR_CFLAGS=`./config/nspr-config --cflags`
  NSPR_VERSION=`./config/nspr-config --version`
  install -m755 -d ${startdir}/pkg/usr/lib/pkgconfig || return 1
  sed ${startdir}/src/nspr.pc.in -e "s,%libdir%,/usr/lib," \
  	-e "s,%prefix%,/usr," \
	-e "s,%exec_prefix%,/usr/bin," \
	-e "s,%includedir%,/usr/include/nspr," \
	-e "s,%NSPR_VERSION%,${NSPR_VERSION}," \
	-e "s,%FULL_NSPR_LIBS%,${NSPR_LIBS}," \
	-e "s,%FULL_NSPR_CFLAGS%,${NSPR_CFLAGS}," > ${startdir}/pkg/usr/lib/pkgconfig/nspr.pc || return 1
  chmod 644 ${startdir}/pkg/usr/lib/pkgconfig/nspr.pc || return 1
  ln -sf nspr.pc ${startdir}/pkg/usr/lib/pkgconfig/mozilla-nspr.pc || return 1

  chmod 644 ${startdir}/pkg/usr/lib/*.a || return 1

  rm -rf ${startdir}/pkg/usr/bin/compile-et.pl \
	${startdir}/pkg/usr/bin/prerr.properties \
	${startdir}/pkg/usr/share/aclocal/nspr.m4 \
	${startdir}/pkg/usr/include/nspr/md
}
