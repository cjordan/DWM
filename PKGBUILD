pkgname=dwm-git
_pkgname=dwm
pkgver=6.0.43.g14343e6
pkgver(){
  cd $_pkgname
  git describe --tags |sed 's/-/./g'
}
pkgrel=1
pkgdesc="A dynamic window manager for X"
url="http://dwm.suckless.org"
arch=('i686' 'x86_64' 'armv7h')
license=('MIT')
options=(zipman)
depends=('libx11' 'libxinerama')
makedepends=('git')
provides=('dwm')
conflicts=('dwm')
epoch=1
source=("$_pkgname::git+http://git.suckless.org/dwm"
        dwm.desktop)
md5sums=('SKIP'
         '939f403a71b6e85261d09fc3412269ee')

build() {
  cd $srcdir/$_pkgname

  for p in ../../patches/0* ../../patches/9*; do
    echo "=> $p"
    patch < $p || return 1
  done

  sed -i 's/CC = cc/CC = clang/g' config.mk
  sed -i 's/-Os/-O2/g' config.mk
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/$_pkgname
  make PREFIX=/usr DESTDIR=$pkgdir install
  install -m644 -D LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE
  install -m644 -D README $pkgdir/usr/share/doc/$pkgname/README
  install -m644 -D $srcdir/dwm.desktop $pkgdir/usr/share/xsessions/dwm.desktop
}
