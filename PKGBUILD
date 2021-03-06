# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Co-Maintainer: Ingo Bürk <ingo.buerk@airblader.de>
# Contributor: Thorsten Töpper <atsutane-aur@freethoughts.de>
# Contributor: William Giokas <1007380@gmail.com>

pkgname=i3lock-fancy-git
pkgver=2.12
pkgrel=1
pkgdesc="An improved screenlocker based upon XCB and PAM"
arch=('i686' 'x86_64')
url="https://i3wm.org/i3lock/"
license=('MIT')
groups=('i3-vcs')
provides=('i3lock')
conflicts=('i3lock')
depends=('xcb-util-image' 'libev' 'cairo' 'libxkbcommon-x11')
backup=("etc/pam.d/i3lock")
options=('docs')
makedepends=('git')
source=('git://github.com/XDjackieXD/i3lock-fancy#branch=master')
sha1sums=('SKIP')

_gitname='i3lock-fancy'

pkgver() {
  cd "$srcdir/$_gitname"
  git describe --tags | sed 's/-/./g'
}

build() {
  cd "$_gitname"

  # Fix ticket FS#31544, sed line taken from gentoo
  sed -i -e 's:login:system-auth:' pam/i3lock

  autoreconf --force --install

  rm -rf build/
  mkdir -p build && cd build/

  ../configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --disable-sanitizers

  # See https://lists.archlinux.org/pipermail/arch-dev-public/2013-April/024776.html
  make CPPFLAGS+="-U_FORTIFY_SOURCE"
}


package() {
  cd "$_gitname"
  cd build/

  make DESTDIR="$pkgdir/" install

  install -Dm644 ../LICENSE \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set ts=2 sw=2 et:
