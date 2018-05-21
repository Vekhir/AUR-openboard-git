# Maintainer: Frank Siegert <frank.siegert@googlemail.com>
pkgname=openboard
pkgver=1.4.0
pkgrel=2
pkgdesc="Interactive whiteboard software for schools and universities"
arch=('x86_64' 'i686')
url="http://openboard.ch/index.en.html"
license=('GPL3')
depends=('qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-script' 'qt5-webkit' 'qt5-tools' 'libpaper' 'bzip2' 'openssl-1.0' 'libfdk-aac')
source=("https://github.com/OpenBoard-org/OpenBoard/archive/v$pkgver.tar.gz"
        "https://github.com/OpenBoard-org/OpenBoard-ThirdParty/archive/master.zip"
        ssl10.patch
        qchar.patch
        ec88fc43aa96bedae1ad32d07e0237fe6fb940b1.patch
        openboard.desktop)
md5sums=('bc04b7178828a6a76dcc7b273276ad30'
         'fa1ff089f0bcc15d2a510bb90cdd3002'
         '9dbccb56e4079b75c606dc40c3e77f00'
         'bf2c524f3897cfcfb4315bcd92d4206e'
         'c8c6821f192993626986e60032d2ea41'
         '21d1749400802f8fc0669feaf77de683')

prepare() {
  rm -rf $srcdir/OpenBoard-ThirdParty
  mv "$srcdir/OpenBoard-ThirdParty-master" "$srcdir/OpenBoard-ThirdParty"

  cd $srcdir/OpenBoard-$pkgver
  patch -p1 < $srcdir/ssl10.patch
  patch -p1 < $srcdir/qchar.patch
  patch -p1 < $srcdir/ec88fc43aa96bedae1ad32d07e0237fe6fb940b1.patch
}

build() {
  cd "$srcdir/OpenBoard-ThirdParty"
  
  cd freetype
  qmake freetype.pro -spec linux-g++
  make
  cd ..

  cd quazip
  qmake quazip.pro -spec linux-g++
  make
  cd ..

  cd xpdf/xpdf-3.04
  ./configure --with-freetype2-library="../../freetype/lib/linux" --with-freetype2-includes="../../freetype/freetype-2.6.1/include"
  cd ..
  qmake xpdf.pro -spec linux-g++
  make
  cd ..

  cd "$srcdir/OpenBoard-$pkgver"
  qmake OpenBoard.pro -spec linux-g++
  make
}

package() {
  cd "$srcdir/OpenBoard-$pkgver"

  mkdir -p $pkgdir/opt/openboard

  for i in customizations etc i18n library; do
    cp -rp $srcdir/OpenBoard-$pkgver/resources/$i $pkgdir/opt/openboard;
  done

  cp -rp $srcdir/OpenBoard-$pkgver/resources/images/OpenBoard.png $pkgdir/opt/openboard/
  cp -rp build/linux/release/product/OpenBoard $pkgdir/opt/openboard/

  mkdir -p $pkgdir/usr/share/applications
  cp -rp $srcdir/openboard.desktop $pkgdir/usr/share/applications

  mkdir -p $pkgdir/usr/bin
  ln -s /opt/openboard/OpenBoard $pkgdir/usr/bin/openboard
}
