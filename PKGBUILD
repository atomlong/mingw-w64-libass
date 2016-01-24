# Maintainer: Karl-Felix Glatzer <karl.glatzer@gmx.de>

pkgname=mingw-w64-libass
pkgver=0.13.1
pkgrel=1
pkgdesc="A portable library for SSA/ASS subtitles rendering (mingw-w64)"
arch=('any')
url="https://github.com/libass/libass/"
license=('BSD')
depends=('mingw-w64-crt' 'mingw-w64-fribidi' 'mingw-w64-fontconfig')
options=(!strip !buildflags !libtool staticlibs)
makedepends=('mingw-w64-gcc' 'mingw-w64-pkg-config' 'mingw-w64-configure' 'yasm')
source=("https://github.com/libass/libass/releases/download/${pkgver}/libass-${pkgver}.tar.xz")
md5sums=('43b5f379e70b3aef15b2435e21e5b73c')
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd ${srcdir}/libass-$pkgver
  autoreconf -ivf
}

build() {
  for _arch in ${_architectures}; do
    mkdir -p ${srcdir}/libass-$pkgver/build-${_arch} && cd ${srcdir}/libass-$pkgver/build-${_arch}

    ${_arch}-configure \
      --enable-harfbuzz \
      --enable-fontconfig
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd ${srcdir}/libass-$pkgver/build-${_arch}

    make DESTDIR="${pkgdir}" install
    #install -D -m644 ${srcdir}/libass-${pkgver}/COPYING "${pkgdir}/usr/${_arch}/share/licenses/libass/LICENSE"
    ${_arch}-strip -x -g ${pkgdir}/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g ${pkgdir}/usr/${_arch}/lib/*.a
  done
}
