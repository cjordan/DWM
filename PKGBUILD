pkgname=dwm-git
_pkgname=dwm
pkgver=6.0.43.g14343e6
pkgrel=1
pkgdesc="A dynamic window manager for X"
url="http://dwm.suckless.org"
arch=('i686' 'x86_64' 'armv7h')
license=('MIT')
options=(zipman)
depends=('libx11' 'libxinerama')
makedepends=('git' 'clang')
provides=('dwm')
conflicts=('dwm')
epoch=2
source=("$_pkgname::git+http://git.suckless.org/dwm#commit=14343e69cc596b847f71f1e825d3019ab1a29aa8"
        dwm.desktop
        push.c)
md5sums=('SKIP'
         '939f403a71b6e85261d09fc3412269ee'
         '689534c579b1782440ddcaf71537d8fd')

pkgver(){
    cd "${_pkgname}"
    git describe --tags |sed 's/-/./g'
}

prepare() {
    cd "${srcdir}/${_pkgname}"

    # Apply patches
    for p in ../../patches/{0..9}*; do
        [ -f "${p}" ] || continue
        echo "=> ${p}"
        patch < ${p} || return 1
    done
    cp ../../push.c .
    # Copy the patched config.def.h to config.h
    cp config.def.h config.h

    # Tweaking make options
    sed \
        -e 's/CPPFLAGS =/CPPFLAGS +=/g' \
        -e 's/CFLAGS   =/CFLAGS   +=/g' \
        -e 's/LDFLAGS =/LDFLAGS +=/g' \
        -e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
        -e 's/-Os//g' \
        -e 's/CC = cc/CC = clang/g' \
        -i config.mk
}

build() {
    cd "${srcdir}/${_pkgname}"
    make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
    cd "${srcdir}/${_pkgname}"
    make PREFIX=/usr DESTDIR=$pkgdir install
    install -m644 -D LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE
    install -m644 -D README $pkgdir/usr/share/doc/$pkgname/README
    install -m644 -D $srcdir/dwm.desktop $pkgdir/usr/share/xsessions/dwm.desktop
}
