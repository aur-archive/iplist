# Maintainer: Romain GALLET < romain.gallet at gmial dot com>
# Contributor: updated by pelle.k
# Contributor: mrbug <devmrbug at google's mail service dot com>
# Contributor: Peace4all. Thanks for his patch here: http://pastebin.com/UT0QNsub
pkgname=iplist
pkgver=0.29
pkgrel=8
pkgdesc="list-based packet handler and blocker which uses the netfilter netlink-queue library (kernel 3 or later)"
arch=('i686' 'x86_64')
url="http://iplist.sourceforge.net"
license=('GPL')
depends=('gcc' 'zlib' 'libnetfilter_queue' 'linux>=3.3' 'iptables')
makedepends=('patch' 'wget')
optdepends=('java-runtime: GUI support')
conflicts=('moblock')
backup=('etc/ipblock.conf' 
	'etc/ipblock.lists')
options=('docs' 'strip' 'zipman')
install=$pkgname.install

source=(http://downloads.sourceforge.net/$pkgname/$pkgname-$pkgver.tar.bz2 
	$pkgname.install rc.ipblock ipblock.patch nfq.patch Peace4all.patch ipblock.service Peace4all-conntrack_fix.patch main.cc.patch)

md5sums=('e1f8186621c5ba79f82cdeffe80429d5'  
	'b3d44ab0cb1cef76d723b1d278906b52' 
	'ab1c1c905f6ee11e03ad6a34ec1ff5fb' 
	'09a4d832ca261dad7f6f73add71ab2a4'
	'c9c427b71dde9c4ba32f1a3bcd0d78bf'
	'bab45a86d145536a5a2398d75b847f7b'
	'8a1a692cea8636881ce7e4327acc1866'
	'5403c68c8b560703bc50a63bb7ef5da4'
	'4b02c1a6574e4fb1e288428f08942552')

build() {
  mkdir -p ${pkgdir}/usr/bin
  mkdir -p ${pkgdir}/usr/sbin
  mkdir -p ${pkgdir}/etc/rc.d
  mkdir -p ${pkgdir}/usr/share/applications
  mkdir -p ${pkgdir}/usr/share/pixmaps
  mkdir -p ${pkgdir}/usr/share/java
  mkdir -p ${pkgdir}/usr/share/man/man8
  mkdir -p ${pkgdir}/var/cache/iplist
  mkdir -p ${pkgdir}/usr/share/doc/iplist

  install -Dm 664 ${srcdir}/$pkgname/ipblock.8 ${pkgdir}/usr/share/man/man8
  install -Dm 664 ${srcdir}/$pkgname/ipblock.conf ${pkgdir}/etc/
  install -Dm 664 ${srcdir}/$pkgname/ipblock.lists ${pkgdir}/etc/
  install -Dm 664 ${srcdir}/$pkgname/allow.p2p ${pkgdir}/var/cache/iplist/
  install -Dm 664 ${srcdir}/$pkgname/ipblock.png ${pkgdir}/usr/share/pixmaps
  install -Dm 664 ${srcdir}/$pkgname/ipblock.desktop ${pkgdir}/usr/share/applications
  install -Dm 664 ${srcdir}/$pkgname/changelog ${pkgdir}/usr/share/doc/iplist
  install -Dm 664 ${srcdir}/$pkgname/COPYING ${pkgdir}/usr/share/doc/iplist
  install -Dm 664 ${srcdir}/$pkgname/INSTALL ${pkgdir}/usr/share/doc/iplist
  if [ -d "/usr/lib/systemd/system" ]; then
     echo "Installing ipblock.service into /usr/lib/systemd/system"
     echo "Run systemctl enable ipblock.service to enable at boot time"
     install -Dm 664 ${srcdir}/ipblock.service ${pkgdir}/usr/lib/systemd/system/ipblock.service
  fi

  echo "Installing legacy startup file rc.ipblock in /etc/rc.d/ipblock"
  install -Dm 755 ${srcdir}/rc.ipblock ${pkgdir}/etc/rc.d/ipblock
  
  
  cd ${srcdir}/$pkgname
  patch --no-backup-if-mismatch ${srcdir}/iplist/src/nfq.cc ${srcdir}/nfq.patch || return 1
  patch -p1   < ../../Peace4all.patch || return 1
  patch -p1   < ../../Peace4all-conntrack_fix.patch || return 1
  patch --no-backup-if-mismatch ${srcdir}/iplist/src/main.cc ${srcdir}/main.cc.patch || return 1
  make || return 1
  make DESTDIR="${pkgdir}" install || return 1
  mv ${pkgdir}/usr/{s,}bin/ipblock || return 1
  mv ${pkgdir}/usr/{s,}bin/iplist || return 1
  rm -fr ${pkgdir}/usr/sbin

  touch ${pkgdir}/var/cache/iplist/allow-perm.p2p
  touch ${pkgdir}/var/cache/iplist/allow-temp.p2p
  patch --no-backup-if-mismatch ${pkgdir}/usr/bin/ipblock ${srcdir}/ipblock.patch || return 1

}

