pkgname=alsa-ucm-conf-radxa-qcom
pkgbasename="alsa-ucm-conf"
pkgver=1.2.14
pkgrel=2
pkgdesc="ALSA Use Case Manager configuration (and topologies), for Radxa QCOM devices"
provides=("alsa-ucm-conf")
conflicts=("alsa-ucm-conf")
arch=(any)
url="https://alsa-project.org/"
license=(BSD)
makedepends=('git')
source=("${pkgbasename}::git+https://github.com/strongtz/alsa-ucm-conf.git#branch=rs782")
sha512sums=('SKIP')
b2sums=('SKIP')

pkgver() {
    cd "${pkgbasename}"
    local _ver
    read -r _ver <VERSION

    local _patchver
    local _patchfile
    for _patchfile in "${source[@]}"; do
        _patchfile="${_patchfile%%::*}"
        _patchfile="${_patchfile##*/}"
        [[ $_patchfile = *.patch ]] || continue
        _patchver="${_patchver}$(md5sum ${srcdir}/${_patchfile} | cut -c1-32)"
    done
    _patchver="$(echo -n $_patchver | md5sum | cut -c1-7)"

    echo ${_ver/-/_}.$(git rev-list --count HEAD).$(git rev-parse --short HEAD).${_patchver}
}

prepare() {
    local _patchfile
    for _patchfile in "${source[@]}"; do
        _patchfile="${_patchfile%%::*}"
        _patchfile="${_patchfile##*/}"
        [[ $_patchfile = *.patch ]] || continue
        echo "Applying patch $_patchfile..."
        patch --directory="${pkgbasename}" --forward --strip=1 --input="${srcdir}/${_patchfile}"
    done
}

package() {
  cd $srcdir/${pkgbasename}
  install -vdm 755 "${pkgdir}/usr/share/alsa/"
  cp -av ucm2 "${pkgdir}/usr/share/alsa/"
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgbasename"
  install -vDm 644 README.md -t "$pkgdir/usr/share/doc/$pkgbasename"
  install -vDm 644 ucm2/README.md -t "$pkgdir/usr/share/doc/$pkgbasename/ucm2"
}
