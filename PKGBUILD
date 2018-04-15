# $Id$
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer:Biginoz < biginoz AT free point fr>
# Contributor: Ignacio Galmarino <igalmarino@gmail.com>
# Contributor: Matthias Sobczyk <matthias.sobczyk@googlemail.com>
# Contributor: Kamil Kaminski <kyle@kkaminsk.com>

_pkgname=minidlna
pkgname=thebox-minidlna
pkgver=1.2.1
pkgrel=2
pkgdesc="A DLNA/UPnP-AV Media server (aka ReadyDLNA) with The Box icons"
arch=('i686' 'x86_64' 'armv7h')
url="http://sourceforge.net/projects/minidlna/"
license=('GPL')
depends=('libexif' 'libjpeg' 'libid3tag' 'flac' 'libvorbis' 'ffmpeg' 'sqlite')
makedepends=('git')
provides=('minidlna')
conflicts=('minidlna' 'minidlna-custom-icon' 'readymedia-transcode-git')
backup=('etc/minidlna.conf')
changelog=changelog
source=("minidlna::git://git.code.sf.net/p/minidlna/git#tag=v${pkgver//./_}"
	minidlna.service
	minidlna.tmpfiles
	minidlna.sysusers
  minidlna-thebox-icons.patch)
sha256sums=('SKIP'
            'dd0fd8f21b49de74c98cd848c820262634cbfb2d26a6fc360eb4455fe1089d1c'
            '63a6edd22f14ba19618e251f4e7117da65ab80c8136466cfe1035cff7e7fde81'
            'b68ae22828629bc0143d2318e5ec58c94d399ec09bdac529972e35f50321628c'
            'SKIP')

prepare() {
  cd "$srcdir/$_pkgname"
  sed -i 's|#user=.*|user=minidlna|g' minidlna.conf
  patch -Np1 -i ../minidlna-thebox-icons.patch
}

build() {
  cd "$srcdir/$_pkgname"
  [ -x configure ] || ./autogen.sh
  ./configure --prefix=/usr --sbindir=/usr/bin
  make
}

package() {
  cd "$srcdir/$_pkgname"
  DESTDIR="$pkgdir" make install
  install -Dm644 minidlna.conf "$pkgdir"/etc/minidlna.conf
  install -Dm0644 "$srcdir"/minidlna.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/minidlna.conf
  install -Dm0644 "$srcdir"/minidlna.sysusers "$pkgdir"/usr/lib/sysusers.d/minidlna.conf
  install -Dm0644 "$srcdir"/minidlna.service "$pkgdir"/usr/lib/systemd/system/minidlna.service
  install -Dm644 "$srcdir"/$_pkgname/minidlna.conf.5 "$pkgdir"/usr/share/man/man5/minidlna.conf.5
  install -Dm644 "$srcdir"/$_pkgname/minidlnad.8 "$pkgdir"/usr/share/man/man8/minidlnad.8
}
