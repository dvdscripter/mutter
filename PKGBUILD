# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.28.2+20+gca4209d88
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://git.gnome.org/browse/mutter"
arch=(x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas libcanberra
         startup-notification zenity libsm gnome-desktop upower libxkbcommon-x11
         gnome-settings-daemon libgudev libinput pipewire)
makedepends=(intltool gobject-introspection git)
groups=(gnome)
_commit=ca4209d88aee9a8d260753bb71a48d85c4a8db75  # gnome-3-28
source=("git+https://gitlab.gnome.org/GNOME/mutter.git#commit=$_commit"
        startup-notification.patch)
sha256sums=('SKIP'
            '5a35ca4794fc361219658d9fae24a3ca21a365f2cb1901702961ac869c759366')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname

  # Port to pipewire 0.2
  git cherry-pick -n 0407a8b33d8c3503fba63ad260984bb08bd6e0dc

  # https://bugs.archlinux.org/task/51940
  # As of 2018-05-08: Still needed, according to fmuellner
  patch -Np1 -i ../startup-notification.patch

  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname

  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
      --libexecdir=/usr/lib --disable-static \
      --disable-schemas-compile --enable-compile-warnings=minimum \
      --enable-gtk-doc --enable-egl-device --enable-remote-desktop

  # https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -e 's/ -shared / -Wl,-O1,--as-needed\0/g' \
      -i {.,cogl,clutter}/libtool

  make
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install
}
