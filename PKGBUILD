pkgname=dwm-git
_pkgname=dwm
pkgver=6.1.r2.g3465bed
pkgrel=1
epoch=2
pkgdesc="A dynamic window manager for X"
url="http://dwm.suckless.org"
arch=("i686" "x86_64" "armv7h")
license=("MIT")
options=(zipman)
depends=("libx11" "libxinerama" "libxft" "freetype2" "st" "dmenu" "terminus-font-ttf")
makedepends=("git")
provides=("dwm")
conflicts=("dwm")
source=("git://git.suckless.org/dwm"
        "dwm.desktop"
        "push.c"
        "01-statuscolors.diff"
        "02-systray.diff"
        "03-cycle.diff"
        "04-better-borders.diff"
        "05-pertag.diff"
        "90-personal.diff"
        "91-betelgeuse.diff"
        "91-pollux.diff")
md5sums=('SKIP'
         '939f403a71b6e85261d09fc3412269ee'
         '689534c579b1782440ddcaf71537d8fd'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP')

pkgver() {
  cd "${_pkgname}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "${srcdir}/${_pkgname}"
    git checkout 3465bed290abc62cb2e69a8096084ba6b8eb4956

    # Apply patches
    for p in "${srcdir}"/{0..8}*.diff; do
        [ -f "${p}" ] || continue
        echo "=> ${p}"
        patch < ${p} || return 1
    done
    # Apply personal customisations
    echo "=> ${srcdir}/90-personal.diff"
    patch < "${srcdir}"/90-personal.diff || return 1
    if [[ -f "${srcdir}"/91-$(hostname).diff ]]; then
        echo "=> ${srcdir}/91-$(hostname).diff"
        patch < "${srcdir}"/91-$(hostname).diff || return 1
    fi

    mv "${srcdir}"/push.c .
    # Copy the patched config.def.h to config.h
    cp config.def.h config.h

    # Tweaking make options
    sed \
        -e 's/CPPFLAGS =/CPPFLAGS +=/g' \
        -e 's/CFLAGS   =/CFLAGS   +=/g' \
        -e 's/LDFLAGS =/LDFLAGS +=/g' \
        -e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
        -e 's/-Os//g' \
        -e 's/CC = cc/CC = gcc/g' \
        -i config.mk
}

build() {
    cd "${srcdir}/${_pkgname}"
    make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11 FREETYPEINC=/usr/include/freetype2
}

package() {
    cd "${srcdir}/${_pkgname}"
    make PREFIX=/usr DESTDIR=$pkgdir install
    install -m644 -D LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE
    install -m644 -D README $pkgdir/usr/share/doc/$pkgname/README
    install -m644 -D $srcdir/dwm.desktop $pkgdir/usr/share/xsessions/dwm.desktop
}
