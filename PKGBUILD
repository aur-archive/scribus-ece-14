# Contributor: Otakar Haška <ota.haska@seznam.cz> 

pkgname=scribus-ece-14
pkgver=8c4425b
pkgrel=1
pkgdesc="A desktop publishing program - Everybody Can Enhance"
arch=(i686 x86_64)
license=('GPL')
url="http://www.scribus-ece.info"
install=${pkgname}.install
depends=('libcups>=1.2.10' 'lcms>=1.17' 'qt4>=4.3.0' \
	'ghostscript>=8.60' 'libart-lgpl>=2.3.19' 'python>=2.5' \
       	'libxml2>=2.6.27' 'cairo' 'desktop-file-utils' 'freetype2>=2.3.0' \
	'libtiff>=3.6.0' 'libjpeg' 'podofo')
makedepends=('subversion' 'cmake' 'gcc>=4.0' 'make')
# options=(!libtool force !makeflags)
replaces=()
source=('scribus-ece-14.desktop')

_gitroot=('git://scribus-ece.info/ScribusECE')
_gitname=('ece14')

pkgver() {
  cd $srcdir/ScribusECE
  git describe --always | sed 's|-|.|g'
}

build() {
  cd $startdir/src
  msg "Connecting to git server...."
  [ ! -d "$srcdir/ScribusECE-build" ] || rm -rf "$srcdir/ScribusECE-build"
  
  if [[ -d ScribusECE ]]; then
   cd ScribusECE || return 1
   git pull origin || return 1
    else
   git clone -b $_gitname  $_gitroot || return 1
     fi
  msg " checkout done."

    msg "Starting make..."
    cd "$srcdir"
    #rm -r "$srcdir/$_gitname-build"
    cp -r $srcdir/ScribusECE ScribusECE-build
    cd ScribusECE-build/Scribus
    
export CMAKE_PREFIX_PATH=/usr/local
export CMAKE_LIBRARY_PATH=/usr/local/lib
export CMAKE_INCLUDE_PATH=/usr/local/include
echo $CMAKE_PREFIX_PATH

  cmake ../Scribus -DCMAKE_INSTALL_PREFIX:PATH=/opt/scribus-ece14 \
		    -DCMAKE_SKIP_RPATH:BOOL=YES \
		    -DWANT_GRAPHICSMAGICK=1 \
		    -DWANT_SYSTEM_CAIRO=1 \
		    -DCMAKE_INCLUDE_PATH=/usr/include/python2.7

  sed -i 's|-Wl,--as-needed||' scribus/CMakeFiles/scribus.dir/link.txt
  make || return 1
}


package() {
cd $srcdir/ScribusECE-build/Scribus
  make DESTDIR=$pkgdir install || return 1

  install -Dm644 ${startdir}/scribus-ece-14.desktop \
    ${pkgdir}/usr/share/applications/scribus-ece-14.desktop

}

md5sums=('77d1e7842739a1d719630346b77734fb')
