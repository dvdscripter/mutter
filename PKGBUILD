# Maintainer: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.1.91.1
pkgrel=2
pkgdesc="A window manager for GNOME"
arch=(i686 x86_64)
license=('GPL')
depends=('startup-notification' 'gconf' 'zenity' 'libcanberra' 'clutter' 'gobject-introspection')
makedepends=('intltool' 'gnome-doc-utils')
url="http://www.gnome.org"
groups=('gnome')
options=('!libtool' '!emptydirs')
install=mutter.install
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/${pkgver%.*.*}/${pkgname}-${pkgver}.tar.xz)
sha256sums=('ea665ba523d2b62f8cdfb1b0285c7672ac395774af3cb059bfd39bed8cb71d31')

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
