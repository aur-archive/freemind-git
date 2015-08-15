# Maintainer: Stefan Husmann <stefan-husmann@t-online.de> 
pkgname=freemind-git
pkgver=20121104
pkgrel=1
pkgdesc="Free mind-mapping software written in Java - git sources"
arch=('any')
url="http://freemind.sourceforge.net/wiki/index.php/Main_Page"
license=('GPL' 'custom:MIT')
depends=('java-runtime' 'bash')
makedepends=('git' 'apache-ant')
provides=('freemind')
conflicts=('freemind' 'freemind-unstable' 'freemind-cvs')
source=("$pkgname.sh" "$pkgname.png" "$pkgname.desktop")
md5sums=('27230ab6e8463c1bcd3de91666d20ec0'
         '92e7ccb968dbe04e0c5acb3352c4847e'
         '9057b541359072bf65ed47503bacd29c')
install=freemind-git.install

_gitroot=git://freemind.git.sourceforge.net/gitroot/freemind/freemind 
_gitname="freemind"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"/freemind

  #
  # BUILD HERE
  #
  
  ant dist
  ant post
}

package() { 
  install -Dm755 ${srcdir}/$pkgname.sh ${pkgdir}/usr/bin/freemind
  cd ${srcdir}/"$_gitname-build"/freemind
  for file in $( find lib plugins -type f ) ; do
    install -Dm644 ${file} ${pkgdir}/usr/share/freemind/${file}
  done
  cd ${srcdir}/"$_gitname-build"/bin/dist
  cp -r plugins ${pkgdir}/usr/share/freemind/
  install -d ${pkgdir}/usr/share/$pkgname/lib/
  cp -r ${srcdir}/"$_gitname-build"/bin/dist/lib/* \
    ${pkgdir}/usr/share/$pkgname/lib/
  install -Dm644 ${srcdir}/"$_gitname-build"/bin/dist/patterns.xml \
    ${pkgdir}/etc/$pkgname/patterns.xml
  install -Dm755 ${srcdir}/"$_gitname-build"/bin/dist/freemind.sh \
    ${pkgdir}/usr/share/$pkgname/freemind.sh
  install -d ${pkgdir}/usr/share/$pkgname/accessories
  install -d ${pkgdir}/usr/share/doc/$pkgname
  install -Dm644 ${srcdir}/"$_gitname-build"/bin/dist/accessories/* \
    ${pkgdir}/usr/share/$pkgname/accessories/
  cp ${srcdir}/"$_gitname-build"/bin/dist/doc/* \
    ${pkgdir}/usr/share/doc/$pkgname
  install -Dm644 ${srcdir}/$pkgname.desktop \
    ${pkgdir}/usr/share/applications/freemind.desktop
  install -Dm644 ${srcdir}/$pkgname.png \
    ${pkgdir}/usr/share/pixmaps/freemind.png
  install -Dm644 ${pkgdir}/usr/share/$pkgname/accessories/LICENSE.MIT \
    ${pkgdir}/usr/share/licenses/$pkgname/LICENSE.MIT.accessories
} 
