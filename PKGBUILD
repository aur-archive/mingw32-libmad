# Contributor: Joe Davison <joedavison.davison@gmail.com>
# Maintainer: Davorin Učakar <davorin.ucakar@gmail.com>

pkgname=mingw32-libmad
pkgver=0.15.1b
pkgrel=3
pkgdesc="A high-quality MPEG audio decoder"
arch=('any')
url="http://www.underbit.com/products/mad/"
license=('GPL')
depends=('mingw32-runtime' 'libmad')
makedepends=('mingw32-gcc')
options=('!buildflags' '!libtool' '!strip')
source=(http://downloads.sourceforge.net/sourceforge/mad/libmad-${pkgver}.tar.gz{,.sign}
        libmad.patch frame_length.diff optimize.diff)
sha1sums=('cac19cd00e1a907f3150cc040ccc077783496d76'
          '24c44ac7c96dca472e7305a7e59f1efd921a3499'
          '5e7369c77de2329f6542ffc4f633eec5a5245091'
          'b9c61ecacc6a6d47425d66f33327e0634cd8a33c'
          '3d5b958244ef0395ccdcb00344f2cf301ca07e34')

build() {
  cd "${srcdir}/libmad-${pkgver}"
  patch -p1 -i "${srcdir}/libmad.patch"
  patch -p1 -i "${srcdir}/frame_length.diff"
  patch -p1 -i "${srcdir}/optimize.diff"

  CFLAGS="-march=i686 -mtune=generic -O2 -s -ftree-vectorize -ftree-vectorizer-verbose=1"
  unset LDFLAGS

  autoconf
  ./configure --prefix=/usr/i486-mingw32 --host=i486-mingw32 --disable-debugging
  make

  # Buildsys doesn't build DLL, no matter what.
  i486-mingw32-gcc $CFLAGS -shared .libs/*.o -o .libs/libmad.dll
}
package() {
  cd "${srcdir}/libmad-${pkgver}"

  make DESTDIR="${pkgdir}" install
  install -Dm755 .libs/libmad.dll "${pkgdir}/usr/i486-mingw32/bin/libmad.dll"

  i486-mingw32-strip -x -g "${pkgdir}"/usr/i486-mingw32/bin/*.dll
  i486-mingw32-strip -g "${pkgdir}"/usr/i486-mingw32/lib/*.a
}
