# Maintainer: Vojtech Kral <vojtech at kral dot hk>

pkgname=qt-demos
pkgver=4.8.4
pkgrel=4
pkgdesc='Qt ships with lots of example programs that teach you how to implement various tasks with Qt'
arch=('i686' 'x86_64')
url='http://qt.nokia.com/'
license=('GPL3' 'LGPL')
depends=("qt4>=${pkgver}")
makedepends=("qt4>=${pkgver}" 'mesa')
optdepends=('qt4-doc: Examples fully documented')
options=('!libtool')

_pkgfqn="qt-everywhere-opensource-src-${pkgver}"
source=("http://releases.qt-project.org/qt4/source/${_pkgfqn}.tar.gz"
        'shadow-build-plugandpaint.patch'
        'qtdemo.desktop')

sha256sums=('33b0e38ba4c54a0cf8a4be8ca5a4584f1c2e0adfecb57b21d9d1d267b0093270'
            '731de0f2f8747564be3bc33b4d03ae072c442887aaecbcb8525317d7ced7f0a7'
            '415fd225e671a36d9a4d64e57b78ae943c8f9af0cabad052ce30ec3e4e7dc8c2')

build()
{
  cd $srcdir/$_pkgfqn

  cp -r include/QtGui/private demos/shared/

  #This patch fixes linking error during shadow build
  patch -p0 < $srcdir/shadow-build-plugandpaint.patch

  cd $srcdir/$_pkgfqn/demos
  qmake-qt4
  make

  cd $srcdir/$_pkgfqn/examples
  qmake-qt4
  make

  cd $srcdir/$_pkgfqn/demos/qtdemo
  qmake-qt4
  make
}

package()
{
  cd $srcdir/$_pkgfqn

  find demos/* -not -name "*.*" -type f -executable -exec install -p -D -m755 '{}' ${pkgdir}'/usr/share/doc/qt4/{}' \;
  find examples/* -not -name "*.*" -type f -executable -exec install -p -D -m755 '{}' ${pkgdir}'/usr/share/doc/qt4/{}' \;
  install -p -D -m755 bin/qtdemo ${pkgdir}/usr/bin/qtdemo
  install -p -D -m644 ${srcdir}/qtdemo.desktop ${pkgdir}/usr/share/applications/qtdemo.desktop
}
