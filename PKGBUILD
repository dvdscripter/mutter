# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.22.0+3+ga9e386e
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://git.gnome.org/browse/mutter"
arch=(i686 x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas
         libcanberra startup-notification zenity libsm gnome-desktop upower
         libxkbcommon-x11 gnome-settings-daemon libgudev libinput)
makedepends=(intltool gobject-introspection git gnome-common)
groups=(gnome)
options=(!emptydirs)
_commit=a9e386e1af76b3110780291462a329ca7ef9ffad  # master
source=("git://git.gnome.org/mutter#commit=$_commit"
        MetaMonitorManagerKms-stop-accounting-for-mode-fla.patch
        MetaMonitorManagerKms-stop-taking-drmModeModeInfov.patch
        meta-monitor-config-Look-at-an-outputs-modes-direc.patch)
sha256sums=('SKIP'
            'fcae247f030ca2b79b42601499493f18765fb37c77e2be9e1dbb1e0865f064f3'
            '543d51bc471fae78e402f33d7606e8806b95874ed40ba910fea5b807f47ef30a'
            '4bbfa7f5f397c1c462ea3ef0018dc5c93c7a2afc28a3253eed6ade17ebde72fc')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname

  # https://bugzilla.gnome.org/show_bug.cgi?id=772176
  patch -Np1 -i ../MetaMonitorManagerKms-stop-accounting-for-mode-fla.patch
  patch -Np1 -i ../MetaMonitorManagerKms-stop-taking-drmModeModeInfov.patch
  patch -Np1 -i ../meta-monitor-config-Look-at-an-outputs-modes-direc.patch

  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname

  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
      --libexecdir=/usr/lib/$pkgname --disable-static \
      --disable-schemas-compile --enable-compile-warnings=minimum \
      --enable-gtk-doc

  #https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -e 's/ -shared / -Wl,-O1,--as-needed\0/g' \
      -i {.,cogl,clutter}/libtool

  make
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install
}
