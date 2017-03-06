# Maintainer: 0xFelix

pkgname=vcontrold-svn
pkgver=r107
pkgrel=1
pkgdesc='Linux Daemon für Vito Kommunikation'
url='https://sourceforge.net/projects/vcontrold'
arch=(i686 x86_64 armv6h armv7h)
license=(GPL)
backup=(etc/vcontrold/vcontrold.xml etc/vcontrold/vito.xml)
depends=(libxml2)
makedepends=(subversion)
source=("${pkgname}::svn+https://svn.code.sf.net/p/vcontrold/code/trunk"
        'vcontrold.service'
        'vcontrold.xml'
        'vito.xml')
sha256sums=('SKIP'
            '1573c77833ceb552b4a2226f2ae712798e62f3e8c96e3de8c7572fbea0ee0b81'
	    'af594d161ea645a568520e39d9da63d2ba02dcdb389ae2c85f0ef57ec7bdb475'
	    '65650ec909e131d8796f56be7e3a9412af7e117e357d7b117b1deb2eb88415cb')

pkgver() {
  cd "${pkgname}/vcontrold"
  printf "r%s" "$(svn info --show-item revision)"
}

build() {
  cd "${pkgname}/vcontrold"
  chmod +x auto-build.sh
  ./auto-build.sh
  ./configure --prefix=/usr
  make
}

package() {
  cd "${pkgname}/vcontrold"
  make DESTDIR="${pkgdir}" sbindir="/usr/bin" install

  cd ../xml-32
  install -dm 755 "${pkgdir}/etc/vcontrold/xml"
  install -m 644 xml/* "${pkgdir}/etc/vcontrold/xml" 
  install -dm 755 "${pkgdir}/etc/vcontrold/xml_p300"
  install -m 644 xml_p300/* "${pkgdir}/etc/vcontrold/xml_p300"
  
  install -dm 755 "${pkgdir}/etc/vcontrold/xml_20c8"
  install -m 644 "${srcdir}/vcontrold.xml" "${pkgdir}/etc/vcontrold/xml_20c8"
  install -m 644 "${srcdir}/vito.xml" "${pkgdir}/etc/vcontrold/xml_20c8"
  ln -s "xml_20c8/vcontrold.xml" "${pkgdir}/etc/vcontrold/vcontrold.xml"
  ln -s "xml_20c8/vito.xml" "${pkgdir}/etc/vcontrold/vito.xml"

  install -Dm 644 "${srcdir}/vcontrold.service" "${pkgdir}/usr/lib/systemd/system/vcontrold.service"
}
