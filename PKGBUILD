# Maintainer: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.0.1
pkgrel=2
pkgdesc="A window manager for GNOME3"
arch=(i686 x86_64)
license=('GPL')
depends=('startup-notification' 'gconf' 'zenity' 'libcanberra' 'clutter' 'gobject-introspection')
makedepends=('intltool' 'gtk-doc')
url="http://www.gnome.org"
groups=('gnome')
options=('!libtool' '!emptydirs')
install=mutter.install
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/${pkgver%.*}/${pkgname}-${pkgver}.tar.bz2
        squash_some_leaks.patch)
sha256sums=('6c3190789f935a2c982e78447726e87cf1d4b7af2f0b407cb6d6aca636e3d708'
            'dda962cfd884ffbe2c3c4a86641964228d7b04ef30e19bb2894c4398fa4c296a')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  patch -Np1 -i "${srcdir}/squash_some_leaks.patch"
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
