# $Id$
# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Georg Vogetseder <georg.vogetseder@gmail.com>
# Contributor: Pablo Recio <rikutheronin@gmail.com>

pkgname=hamster-standalone
pkgver=2.32.1
pkgrel=2
pkgdesc="GNOME time tracking standalone application without the applet"
arch=('any')
url="http://projecthamster.wordpress.com/"
license=('GPL')
depends=('python2-libgnome' 'python2-gconf' 'python2-gnomecanvas' 'dbus-python>=0.83.1' 'python-wnck>=2.32' 'python-pysqlite>=2.6.0' 'pyxdg')
makedepends=('intltool' 'gnome-doc-utils>=0.20.1' 'gnome-control-center')
options=('!libtool' '!emptydirs')
groups=('gnome-extra')
install=hamster-standalone.install
source=(ftp://ftp.gnome.org/pub/GNOME/sources/hamster-applet/2.32/hamster-applet-${pkgver}.tar.bz2)
sha256sums=('98302221efd4ccd4a2446d2cda15b795a0b23850f562935a3dd61afe3e2cd0aa')

build() {
  cd "${srcdir}/hamster-applet-${pkgver}"
  sed -i "s|#!/usr/bin/env python$|#!/usr/bin/env python2|" waf

  ./waf --prefix=/usr configure build
  ./waf install --destdir=${pkgdir} --libdir=/usr/lib \
      --libexecdir=/usr/lib/hamster-applet \
      --sysconfdir=/etc \
      --localstatedir=/var

  sed -i "s|#!/usr/bin/python$|#!/usr/bin/python2|" $pkgdir/usr/bin/hamster-service

  sed -i "s|#!/usr/bin/env python$|#!/usr/bin/env python2|" \
    $pkgdir/usr/share/docky/helpers/hamster_control.py \
    $pkgdir/usr/share/dockmanager/scripts/hamster_control.py \
    $pkgdir/usr/bin/{hamster-cli,{gnome,hamster}-time-tracker}

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/hamster-applet.schemas" --domain hamster-applet ${pkgdir}/etc/gconf/schemas/*.schemas
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
