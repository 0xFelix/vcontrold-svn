# Maintainer: 0xFelix

pkgname=vcontrold-svn
pkgver=r107
pkgrel=2
pkgdesc='Linux Daemon für Vito Kommunikation'
url='https://sourceforge.net/projects/vcontrold'
arch=(i686 x86_64 armv6h armv7h)
license=(GPL)
backup=(etc/vcontrold/vcontrold.xml etc/vcontrold/vito.xml)
depends=(libxml2)
makedepends=(subversion)
provides=(vcontrold)
source=("${pkgname}::svn+https://svn.code.sf.net/p/vcontrold/code/trunk"
        'vcontrold.service'
        'vcontrold.xml'
        'vito.xml')
sha256sums=('SKIP'
            '1573c77833ceb552b4a2226f2ae712798e62f3e8c96e3de8c7572fbea0ee0b81'
	    'bd2da6533147e0011ba0acdde8b45f84c8127f2bac31efdf0d2d9cfcfc0445bc'
	    '1606d8ee4a105cde725a59d9b38d910fca91b282e7ae257cf10adabc0c2bb0a7')

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
