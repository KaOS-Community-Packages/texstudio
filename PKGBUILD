pkgname=texstudio
pkgver=2.12.8
pkgrel=1
pkgdesc="Fork of TexMaker built with Qt5/Phonon that gives you an easy environment for all kind of LaTeX documents"
arch=('x86_64')
url="http://texstudio.sourceforge.net/"
license=('GPLv2')
depends=('poppler' 'phonon-qt5' 'qt5-svg' 'qt5-script' 'texlive-core')
makedepends=('qt5-tools')

source=("https://github.com/texstudio-org/${pkgname}/archive/${pkgver}.tar.gz")
md5sums=('b637e5b1c39613c37031ba92e80b2909')
build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  # Fix .desktop item.
  sed -i -e '/^Encoding/d' -e "/^Icon/s|=.*|=${pkgname}|" utilities/${pkgname}.desktop
  qmake-qt5 PREFIX=/usr QMAKE_CFLAGS+="${CFLAGS}" QMAKE_CXXFLAGS+="${CXXFLAGS}" \
    PHONON=true INCLUDEPATH+="/usr/include/poppler/qt5" \
    CONFIG-="debug" -o Makefile texstudio.pro
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make INSTALL_ROOT="$pkgdir" install
  for _i in 16 22 32 48 64 128; do
    install -D -m 0644 utilities/${pkgname}${_i}x${_i}.png \
      "${pkgdir}"/usr/share/icons/hicolor/${_i}x${_i}/apps/${pkgname}.png
  done
  install -D -m 0644 utilities/${pkgname}.svg "${pkgdir}"/usr/share/icons/hicolor/scalable/apps/${pkgname}.svg
}
