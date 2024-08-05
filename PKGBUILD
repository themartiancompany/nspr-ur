# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>

_os="$( \
  uname \
    -o)"
_arch="$( \
  uname \
    -m)"
_pkg="nspr"
pkgname="${_pkg}"
pkgver=4.35
pkgrel=3
pkgdesc="Netscape Portable Runtime"
url="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR"
arch=(
  x86_64
  arm
  aarch64
  mips
  armv7l
  i686
  pentium4
  powerpc
)
license=(
  MPL-2.0
)
depends=(
)
if [[ "${_os}" == "GNU/Linux" ]]; then
  depends+=(
    glibc
    sh
  )
elif [[ "${_os}" == "Android" ]]; then
  depends+=(
    ndk-sysroot
    bash
  )
fi
makedepends=(
  mercurial
  zip
)
provides=(
  "lib${_pkg}=${pkgver}"
)
conflicts=(
  "lib${_pkg}"
)
_url="https://hg.mozilla.org/projects/${_pkg}"
source=(
  "hg+${_url}#tag=NSPR_${pkgver//./_}_RTM"
  '0001-linux-prefer-GCC-provided-atomics-to-asssembly-imple.patch'
  '0002-configure.in-Remove-assembly-files-from-build.patch'
)
if [[ "${_os}" == "Android" ]]; then
  source+=(
    '0003-configure.in-Android-native-build.patch'
  )
fi
b2sums=(
  '07fe5d0ad41d37080fb3114386490c5ff1d8f7fda406bae3c76118aff0c7007f4c3ebf94162c5c75bf6426a4c1bd0fcce2e3322b87eabfc26a652bc5e3d08dd0'
  'fe81bbb23478958438e385ec5563842cdaf7400021def0d2f2184c0c38389e75f28ed7a4f3b52cada4d76c6318c104dda661f1d4efaa224bc832a989729ef852'
  '1fd6e9b1f3111a29a052b6034f796e4e9577a3dbb2d0e96798ce1f47b74f515c882c9f595198fa1646648611525b48857b33ed62e713991e2f28850690e99060'
  '044d6c36f6a55617f770fc1bb773eab568a66f952db85e0e643cccddbed66a8bf9352be745191232fb23be5abd5419713b56fcac932877326c0c69cef36e39fd'
)

prepare() {
  cd \
    "${_pkg}"
  # https://bugzilla.mozilla.org/show_bug.cgi?id=1496426
  # https://gitlab.archlinux.org/archlinux/packaging/packages/nspr/-/merge_requests/1
  patch \
    -Np1 \
    -i \
    ../0001-linux-prefer-GCC-provided-atomics-to-asssembly-imple.patch
  patch \
    -Np1 \
    -i \
    ../0002-configure.in-Remove-assembly-files-from-build.patch
  if [[ "${_os}" == "Android" ]]; then
    patch \
      -Np1 \
      -i \
      ../0003-configure.in-Android-native-build.patch
  fi
  autoreconf \
    -fvi
}

build() {
  local \
    configure_options=()
  configure_options=(
      --prefix=/usr
      --libdir=/usr/lib
      --includedir="/usr/include/${_pkg}"
      --enable-optimize
      --disable-debug
  )

      --enable-64bit
  if [[ "${_arch}" == "x86_64" ]] || \
     [[ "${_arch}" == "aarch64" ]]; then
    configure_options+=(
      --enable-64bit
    )
  fi
  if [[ "${_os}" == "Android" ]]; then
    _cppflags=(
      "${CPPFLAGS}" 
      "-DANDROID"
    )
  fi
  cd \
    "${_pkg}"
  CPPFLAGS="${_cppflags[*]}" \
  ./configure \
    "${configure_options[@]}"
  CPPFLAGS="${_cppflags[*]}" \
  make \
    ${SOURCE_DATE_EPOCH:+
    SH_NOW="${SOURCE_DATE_EPOCH}000000"
    SH_DATE="$(date --utc --date="@$SOURCE_DATE_EPOCH" '+%Y-%m-%d %T')"
  }
}

package() {
  cd \
    "${_pkg}"
  make \
    DESTDIR="${pkgdir}" \
    install
  ln \
    -s \
    "${_pkg}.pc" \
    "${pkgdir}/usr/lib/pkgconfig/mozilla-${_pkg}.pc"
  rm \
    -r \
    "${pkgdir}/usr/include/${_pkg}/md"
  rm \
    "${pkgdir}"/usr/bin/{compile-et.pl,prerr.properties}
}

# vim:set sw=2 sts=-1 et:
