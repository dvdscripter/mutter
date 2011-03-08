# Maintainer: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=2.91.91
pkgrel=1
pkgdesc="A window manager for GNOME3"
arch=(i686 x86_64)
license=('GPL')
depends=('startup-notification' 'gconf' 'zenity' 'libcanberra' 'clutter' 'gobject-introspection')
makedepends=('intltool' 'gtk-doc')
url="http://www.gnome.org"
groups=('gnome')
options=('!libtool' '!emptydirs')
install=mutter.install
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/${pkgver%.*}/${pkgname}-${pkgver}.tar.bz2)
sha256sums=('2b894a9f57891853438b8f5b774bfb4b985e482c2a8dbdffa01398aa9139cb3d')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/mutter \
      --localstatedir=/var --disable-static
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain mutter ${pkgdir}/etc/gconf/schemas/*.schemas
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
