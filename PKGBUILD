# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.16.1
pkgrel=2
pkgdesc="A window manager for GNOME"
arch=(i686 x86_64)
license=('GPL')
depends=('clutter' 'dconf' 'gobject-introspection-runtime' 'gsettings-desktop-schemas' 'libcanberra' 'startup-notification' 'zenity' 'libsm' 'gnome-desktop' 'upower' 'libxkbcommon-x11' 'gnome-settings-daemon')
makedepends=('intltool' 'libxkbcommon-x11' 'gobject-introspection')
conflicts=('mutter-wayland')
replaces=('mutter-wayland')
url="http://www.gnome.org"
groups=('gnome')
options=('!emptydirs')
install=mutter.install
source=(http://ftp.gnome.org/pub/gnome/sources/$pkgname/${pkgver:0:4}/$pkgname-$pkgver.tar.xz
        revert.patch)
sha256sums=('be487f92bfa60c88fe474bd99f9665d57506479a06a48e00a7dd3171029b701c'
            'ec06d41973e82a608d9de7e7369ad323a58f6f644016657ac54ce226321a6570')

prepare() {
  cd "$pkgname-$pkgver"
  
  # Revert input-settings: Ensure that we always apply the same set of settings
  # because it asserts when VT switching
  patch -Np1 -i ../revert.patch
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
