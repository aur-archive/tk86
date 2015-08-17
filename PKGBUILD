# Maintainer: Vorzard <vorzard at plexomat dot com>

pkgname=tk86
pkgver=8.6.0
pkgrel=1
provides=('tk=8.6')
conflicts=('tk')
pkgdesc="A widget toolkit for use with Tcl"
url="http://tcl.tk/"
arch=('i686' 'x86_64')
license=('custom')
depends=('tcl86' 'libxss' 'libxft')
source=("http://downloads.sourceforge.net/sourceforge/tcl/tk$pkgver-src.tar.gz")
sha1sums=('c42e160285e2d26eae8c2a1e6c6f86db4fa7663b')

build() {
  cd $srcdir/tk$pkgver/unix
  if [ "$CARCH" = "x86_64" ]; then
    ./configure --prefix=/usr --mandir=/usr/share/man --enable-64bit
  else
    ./configure --prefix=/usr --mandir=/usr/share/man --disable-64bit
  fi
  make
}

package() {
  cd $srcdir/tk$pkgver/unix
  make DESTDIR="$pkgdir" install install-private-headers
  ln -sf wish8.6 $pkgdir/usr/bin/wish
  install -Dm644 license.terms $pkgdir/usr/share/licenses/tk/LICENSE

  # Install private headers
  cd $srcdir/tk$pkgver
  for directory in compat generic generic/ttk unix; do
    install -dm755 $pkgdir/usr/include/tk-private/$directory
    install -m644 -t $pkgdir/usr/include/tk-private/$directory $directory/*.h
  done

  # Remove buildroot traces
  sed -i \
    -e "s,^TK_BUILD_LIB_SPEC='-L.*/unix,TK_BUILD_LIB_SPEC='-L/usr/lib," \
    -e "s,^TK_SRC_DIR='.*',TK_SRC_DIR='/usr/include'," \
    -e "s,^TK_BUILD_STUB_LIB_SPEC='-L.*/unix,TK_BUILD_STUB_LIB_SPEC='-L/usr/lib," \
    -e "s,^TK_BUILD_STUB_LIB_PATH='.*/unix,TK_BUILD_STUB_LIB_PATH='/usr/lib," \
    $pkgdir/usr/lib/tkConfig.sh
}
