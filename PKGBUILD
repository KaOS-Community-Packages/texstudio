pkgname=texstudio
pkgver=2.10.8
pkgrel=1
pkgdesc="Fork of TexMaker built with Qt5/Phonon that gives you an easy environment for all kind of LaTeX documents"
arch=('x86_64')
url="http://texstudio.sourceforge.net/"
license=('GPLv2')
depends=('poppler-qt5' 'phonon-qt5' 'qt5-svg' 'qt5-script')
makedepends=('sed' 'qt5-tools')
optdepends=('okular')
install=texstudio.install
source=("http://downloads.sourceforge.net/${pkgname}/${pkgname}-${pkgver}.tar.gz"
        'archlinux-phonon.patch')
md5sums=('a14008ff4d23f33ae2f14dbb4efdcc35'
         '2b4bd3232e9f92271856f7daa318e6c7')

build() {
  cd "${srcdir}/${pkgname}${pkgver}/pdfviewer"
  # Patch include path for phonon detection on KaOS.
  patch -p1 < "${srcdir}"/archlinux-phonon.patch
  # Fix .desktop item.
  cd "${srcdir}/${pkgname}${pkgver}"
  sed -i -e '/^Encoding/d' -e "/^Icon/s|=.*|=${pkgname}|" utilities/${pkgname}.desktop
  qmake-qt5 PREFIX=/usr QMAKE_CFLAGS+="${CFLAGS}" QMAKE_CXXFLAGS+="${CXXFLAGS}" \
    PHONON=true INCLUDEPATH+="/usr/include/poppler/qt5" \
    CONFIG-="debug" -o Makefile texstudio.pro
  make
}

package() {
  cd "${srcdir}/${pkgname}${pkgver}"
  make INSTALL_ROOT="$pkgdir" install
  for _i in 16 22 32 48 64 128; do
    install -D -m 0644 utilities/${pkgname}${_i}x${_i}.png \
      "${pkgdir}"/usr/share/icons/hicolor/${_i}x${_i}/apps/${pkgname}.png
  done
  install -D -m 0644 utilities/${pkgname}.svg "${pkgdir}"/usr/share/icons/hicolor/scalable/apps/${pkgname}.svg
}
