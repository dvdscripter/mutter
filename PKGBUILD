# Maintainer : Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=2.29.0
pkgrel=1
pkgdesc="The next-gen WM for gnome, with compositing by clutter"
arch=('i686' 'x86_64')
license=('GPL')
depends=('startup-notification' 'gconf>=2.28.0' 'zenity>=2.28.0' 'gobject-introspection' 'clutter' 'libcanberra')
makedepends=('intltool' 'pkgconfig' 'gnome-doc-utils>=0.19.5' 'gir-repository')
url="http://www.gnome.org"
options=('!libtool' '!emptydirs')
install=mutter.install
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.29/${pkgname}-${pkgver}.tar.bz2)
sha256sums=('7e4c1c46542f1251e9ec495729a3b753e5753ecd18fe35df586e474f6bf5ab90')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/mutter \
      --localstatedir=/var --disable-static || return 1
  make || return 1
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install || return 1

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain mutter ${pkgdir}/etc/gconf/schemas/*.schemas || return 1
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
