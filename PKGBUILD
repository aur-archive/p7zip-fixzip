# Maintainer: Nohappiness <nohappiness@gmail.com>
# Contributor: Thayer Williams <thayer@archlinux.org>
# Contributor: Hugo Doria <hugo@archlinux.org>
# Contributor: TuxSpirit<tuxspirit@archlinux.fr>  2007/11/17 21:22:36 UTC
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Jianfeng Zhang <swordfeng@orion.codes>

pkgname=p7zip-fixzip
_pkgname=p7zip
pkgver=9.20.1
pkgrel=9
pkgdesc='Command-line version of the 7zip compressed file archiver, patched with natspec (should support utf8)'
url='http://p7zip.sourceforge.net/'
license=('GPL' 'custom')
arch=('i686' 'x86_64')
depends=('gcc-libs' 'bash' 'libnatspec')
provides=('p7zip')
conflicts=('p7zip')
optdepends=('wxgtk2.8: GUI'
		'desktop-file-utils: desktop entries')
makedepends=('yasm' 'nasm' 'wxgtk2.8')
options=('!makeflags')
source=("http://downloads.sourceforge.net/project/${_pkgname}/${_pkgname}/${pkgver}/${_pkgname}_${pkgver}_src_all.tar.bz2"
		'7zFM.desktop'
		'patchfile')
sha1sums=('1cd567e043ee054bf08244ce15f32cb3258306b7'
          'f2c370d6f1b286b7ce9a2804e22541b755616a40'
          '705d124357dc65d4e96105a8ed70b04ae2da57fa')

install=install
prepare() {
	cd "${srcdir}/${_pkgname}_${pkgver}"
	patch -Np1 -i ../patchfile
	[[ $CARCH = x86_64 ]] \
	&& cp makefile.linux_amd64_asm makefile.machine \
	|| cp makefile.linux_x86_asm_gcc_4.X makefile.machine
	sed -i 's/wx-config/wx-config-2.8/g' CPP/7zip/TEST/TestUI/makefile CPP/7zip/UI/{FileManager,GUI,P7ZIP}/makefile
	sed -i 's/-lpthread/-lpthread -lnatspec/g' makefile*
}


build() {
	cd "${srcdir}/${_pkgname}_${pkgver}"
	make all4 OPTFLAGS="${CXXFLAGS}"
}

package() {
	cd "${srcdir}/${_pkgname}_${pkgver}"

	make install \
	DEST_DIR="${pkgdir}" \
	DEST_HOME="/usr" \
	DEST_MAN="/usr/share/man"

	# Licenses
	install -d "${pkgdir}"/usr/share/licenses/p7zip
	ln -s -t "${pkgdir}"/usr/share/licenses/p7zip \
		/usr/share/doc/p7zip/DOCS/License.txt \
		/usr/share/doc/p7zip/DOCS/unRarLicense.txt

	# Integration with stuff...
	install -D GUI/p7zip_32.png "${pkgdir}"/usr/share/icons/hicolor/32x32/apps/p7zip.png
	install -d "${pkgdir}"/usr/share/{applications,kde4/services/ServiceMenus}
	cp GUI/kde4/* "${pkgdir}"/usr/share/kde4/services/ServiceMenus/
	cp ../7zFM.desktop "${pkgdir}"/usr/share/applications/
	ln -s 7zCon.sfx "${pkgdir}"/usr/lib/p7zip/7z.sfx

	find GUI/help -type d -exec chmod 755 {} \;
	cp -r GUI/help "${pkgdir}"/usr/lib/p7zip/

	chmod -R u+w "${pkgdir}/usr"
}
