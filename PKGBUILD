# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.32.0
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas libcanberra
         startup-notification zenity libsm gnome-desktop upower libxkbcommon-x11
         gnome-settings-daemon libgudev libinput pipewire xorg-server-xwayland)
makedepends=(gobject-introspection git egl-wayland meson xorg-server)
checkdepends=(xorg-server-xvfb)
groups=(gnome)
_commit=efb1ee97308653a28ed4448b0c405e6faf2c4f40  # tags/3.32.0^0
source=("git+https://gitlab.gnome.org/GNOME/mutter.git#commit=$_commit"
        216.patch)
sha256sums=('SKIP'
            'ed4f3cf738a3cffdf8a6e1a352bf24d74078c3b26fb9262c5746e0d95b9df756')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname

  # https://gitlab.gnome.org/GNOME/mutter/merge_requests/216
  git apply -3 ../216.patch
}

build() {
  arch-meson $pkgname build \
    -D egl_device=true \
    -D wayland_eglstream=true \
    -D installed_tests=false
  ninja -C build
}

check() (
  mkdir -p -m 700 "${XDG_RUNTIME_DIR:=$PWD/runtime-dir}"
  glib-compile-schemas "${GSETTINGS_SCHEMA_DIR:=$PWD/build/data}"
  export XDG_RUNTIME_DIR GSETTINGS_SCHEMA_DIR

  dbus-run-session xvfb-run -s '+iglx -noreset' meson test -C build
)

package() {
  DESTDIR="$pkgdir" meson install -C build
}
