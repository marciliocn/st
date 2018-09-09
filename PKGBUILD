# Maintainer: Ryan Kes <alrayyes gmail com>

pkgname=st
pkgver=0.8.1
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'libxext' 'xorg-fonts-misc')
makedepends=('ncurses')
url="http://st.suckless.org"

_patches=("https://st.suckless.org/patches/clipboard/st-clipboard-0.8.1.diff"
          "https://st.suckless.org/patches/scrollback/st-scrollback-0.8.diff"
          "https://st.suckless.org/patches/scrollback/st-scrollback-mouse-0.8.diff"
          "https://st.suckless.org/patches/vertcenter/st-vertcenter-20180320-6ac8c8a.diff"
          "https://st.suckless.org/patches/alpha/st-alpha-0.8.1.diff"
          "https://st.suckless.org/patches/disable_bold_italic_fonts/st-disable-bold-italic-fonts.diff"
          "https://st.suckless.org/patches/solarized/st-solarized-dark-20180411-041912a.diff")

source=("http://dl.suckless.org/st/$pkgname-$pkgver.tar.gz"
        "config.h"
        "${_patches[@]}")

sha256sums=('c4fb0fe2b8d2d3bd5e72763e80a8ae05b7d44dbac8f8e3bb18ef0161c7266926'
            '314afa7c29cfd4a782eb93d5c3db6845b65d4bca1b22946c71eb5251dfd0551c'
            'f22e0165aacb2bc86d000728c81f68022abcc601dbfd09e516e1ba772225d7e6'
            '8279d347c70bc9b36f450ba15e1fd9ff62eedf49ce9258c35d7f1cfe38cca226'
            '3fb38940cc3bad3f9cd1e2a0796ebd0e48950a07860ecf8523a5afd0cd1b5a44'
            '04e6a4696293f668260b2f54a7240e379dbfabbc209de07bd5d4d57e9f513360'
            '7bf61cb8a505891574f3ad0a5420334d3e965b9f7d0067df3819eeef72ce1358'
            '206ac98601adead3139eac64a23e28202879b94d6cf61e71e5bf457bde34ed18'
            'b2d5e88a2616eafb82b2fefb63eecb0f9d71f839349ef40f9f69c1953444f88c')

prepare() {
  cd $srcdir/$pkgname-$pkgver
  # skip terminfo which conflicts with nsurses
  sed -i '/\ttic -sx st.info/d' Makefile

  # Modify alpha patch to prevent conflicts
  sed -i 's/size_t colornamelen/unsigned int tabspaces/g' "$srcdir/$(basename ${_patches[4]})"
  sed -i '7,7d' "$srcdir/$(basename ${_patches[5]})" 
  sed -i '10,26d' "$srcdir/$(basename ${_patches[5]})" 
  sed -i '1,68d' "$srcdir/$(basename ${_patches[6]})" 

  for patch in "${_patches[@]}"; do
    echo "Applying patch $(basename $patch)..."
    patch -Np1 -i "$srcdir/$(basename $patch)"
  done

  cp $srcdir/config.h config.h
}

build() {
  cd $srcdir/$pkgname-$pkgver
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make PREFIX=/usr DESTDIR="$pkgdir" TERMINFO="$pkgdir/usr/share/terminfo" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/$pkgname/README"
}
