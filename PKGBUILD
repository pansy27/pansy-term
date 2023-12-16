# Maintainer: pansy <uselessshogun99@gmail.com>
pkgname=pansy-term
_pkgname=rxvt-unicode
pkgver=9.30
pkgrel=1
pkgdesc="rxvt-unicode with true color, enhanced glyphs and fixed layout size patch"
arch=('x86_64')
url="http://software.schmorp.de/pkg/rxvt-unicode.html"
license=('GPL')
depends=(
	'libxft'
    'libxt'
    'perl'
    'startup-notification'
    'libptytty'
)
optdepends=(
	'libxft-bgra: support for BGRA glyphs and scaling'
)
provides=(
	'rxvt-unicode'
    'rxvt-unicode-terminfo'
    'urxvt-resize-font'
    'urxvt-keyboard-select'
)
conflicts=(
    'rxvt-unicode'
    'rxvt-unicode-terminfo'
    'urxvt-resize-font'
    'urxvt-perls'
    'urxvt-perls-git'
)
source=(
    http://dist.schmorp.de/$_pkgname/Attic/$_pkgname-$pkgver.tar.bz2
    'urxvt.desktop'
    'urxvtc.desktop'
    'resize-font'
    'keyboard-select'
    '24-bit-color.patch'
    'enable-wide-glyphs.patch'
  	'fixed-layout-size.patch'
    'improve-font-rendering.patch'
)
sha1sums=(
	'700265a255eedf0f553cadfe5484bf71f8fb74c2'
	'b5a4507f85ebb7bac589db2e07d9bc40106720d9'
	'62c4ffecfce6967def394dd4d418b68652372ea2'
	'a61366659c73bd551fa99a8415bb71e033897598'
	'9883d0c31b45f8521829ea6a2041f2e9eb7abe6a'
	'b5a239179a6da062bcc9c5a36e870387080372d2'
	'bce4b2ae0dfd7e9e0c72ec89ce98b48c511816f6'
	'5755f2bb9a6dac55a9848d0803e97fa90cf2d46a'
	'b23c671b6b6f172189ffd61f0b94efe5e776c978'
)

prepare() {
	cd "$_pkgname-$pkgver"
	patch -p0 -i ../24-bit-color.patch
	patch -p1 -i ../enable-wide-glyphs.patch
	patch -p1 -i ../fixed-layout-size.patch
	patch -p1 -i ../improve-font-rendering.patch
}

build() {
	cd "$_pkgname-$pkgver"
    # disable smart-resize (FS#34807)
    # do not specify --with-terminfo (FS#46424)
	./configure \
        --prefix=/usr \
        --enable-256-color \
        --enable-24-bit-color \
        --enable-unicode3 \
        --enable-combining \
        --enable-xft \
        --enable-font-styles \
        --enable-wide-glyphs \
        --enable-pixbuf \
        --enable-startup-notification \
        --disable-fallback \
        --enable-transparency \
        --enable-fading \
        --disable-rxvt-scroll \
        --disable-next-scroll \
        --disable-xterm-scroll \
        --enable-xim \
        --enable-frills \
        --disable-iso14755 \
        --disable-keepscrolling \
        --enable-selectionscrolling \
        --enable-mousewheel \
        --disable-slipwheeling \
        --disable-smart-resize \
        --enable-text-blink \
        --enable-pointer-blank \
        --enable-perl

	make
}

package() {
    # install freedesktop menu
    for _f in urxvt urxvtc; do
        install -Dm 644 $_f.desktop "$pkgdir/usr/share/applications/$_f.desktop"
    done

    # install perl script resize-font (https://github.com/simmel/urxvt-resize-font)
    install -Dm 644 resize-font "$pkgdir/usr/lib/urxvt/perl/resize-font"

    # install perl script keyboard-select (https://github.com/muennich/urxvt-perls)
    install -Dm 644 keyboard-select "$pkgdir/usr/lib/urxvt/perl/keyboard-select"

	cd "$_pkgname-$pkgver"

    # install terminfo
    export TERMINFO="$pkgdir/usr/share/terminfo"
    install -dm 755 "$TERMINFO"

	make DESTDIR="$pkgdir" install
}
