# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.10.4
pkgrel=1
pkgdesc="A window manager for GNOME"
arch=(i686 x86_64)
license=('GPL')
depends=('clutter' 'dconf' 'gobject-introspection' 'gsettings-desktop-schemas' 'libcanberra' 'startup-notification' 'zenity' 'libsm' 'gnome-desktop' 'upower')
makedepends=('intltool' 'gnome-doc-utils')
url="http://www.gnome.org"
groups=('gnome')
options=('!emptydirs')
install=mutter.install
source=(http://ftp.gnome.org/pub/gnome/sources/$pkgname/${pkgver:0:4}/$pkgname-$pkgver.tar.xz
        0001-monitor-expose-min-backlight-step.patch)
sha256sums=('9159c40ea9f5c5e3d1e67cc12ebcbd6328a7b732274195b4e5bdacb3cb1771e6'
            'f1ef173d96ac27abdcf765972d2ac434f63bf4629b8bd51c177e8963d07f5eb7')

prepare() {
  cd "$pkgname-$pkgver"

  # FS#37224
  patch -Np1 -i ../0001-monitor-expose-min-backlight-step.patch
}

build() {
  cd "$pkgname-$pkgver"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/mutter \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile --enable-compile-warnings=minimum

  #https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package() {
  cd "$pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
}
