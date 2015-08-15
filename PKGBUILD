# Maintainer: Cravix < dr dot neemous at gmail dot org >
# Contributor: Ronald van Haren <ronald.archlinux.org>

pkgname=e-modules-extra-svn-repacker
pkgver=80224
pkgrel=2
pkgdesc="E17 extra modules repacker"
arch=('i686' 'x86_64')
groups=('e17-extra-svn')
url="http://www.enlightenment.org"
license=('GPLv2' 'LGPLv2' 'MIT')
depends=('enlightenment17' 'efreet' 'e_dbus' 'e-modules-extra-svn')
makedepends=('subversion')
conflicts=('e-modules-extra')
provides=('e-modules-extra')
options=('!libtool')

build() {
	sed -i "/^pkgname=/s/-repacker//; /^depends=/s/ 'e-modules-extra-svn'//; 
	 /^pkgver/s/=.*$/=$(pacman -Q e-modules-extra-svn |cut -d" " -f2 |cut -d- -f1 2>/dev/null)/;
	 /^pkgrel/s/=.*$/=$(($(pacman -Q e-modules-extra-svn |cut -d- -f5 2>/dev/null)+1))/ " "$startdir/PKGBUILD"
	# need to restart build after rename
	sed -i "/dissign/s/^/#/" "$startdir/PKGBUILD" && echo "$(tput setaf 1)$(tput bold)Adjustment finished. Now, restart build!" && exit 1
}

package() {
	# retrieve dirname
	# don't know how to directly read anyone of them...
	e17ver=$(pacman -Qlq enlightenment17 enlightenment17-svn 2>/dev/null | sed -n '/enlightenment.*linux-gnu/s/.*\(linux-gnu[^\/]*\).*/\1/p' |head -n1)
	emodver=$(pacman -Qlq e-modules-extra-svn | sed -n '/enlightenment.*linux-gnu/s/.*\(linux-gnu[^\/]*\).*/\1/p' |head -n1)
	
	# copy all files of current e-modules-extra-svn
	for i in $(pacman -Qlq e-modules-extra-svn); do
		[[ -f $i ]] && cp --parents $i ${pkgdir}/
	done

	# changing the dirname (the main work here)
	cd ${pkgdir}/usr/lib/enlightenment/modules
	for i in *; do
		[[ -d $i ]] && rename "$emodver" "$e17ver" $i/linux-gnu*
	done
}
