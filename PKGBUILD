pkgname=alsa-ucm-conf-radxa-qcom
pkgbasename="alsa-ucm-conf"
pkgver=1.2.16.1097.18d142f.0031354
pkgrel=1
pkgdesc="ALSA Use Case Manager configuration (and topologies), for Radxa QCOM devices"
provides=("alsa-ucm-conf")
conflicts=("alsa-ucm-conf")
arch=(any)
url="https://alsa-project.org/"
license=(BSD)
makedepends=('git')
# Patches imported from https://github.com/radxa-pkg/alsa-ucm-conf/tree/31f866c85152e0685e658d13dc4335f01e1c7aeb/debian/patches/radxa
source=(
    "${pkgbasename}::git+https://github.com/alsa-project/alsa-ucm-conf.git#branch=master"
    "0001-remove-exec-bit.patch"
    "0003-ucm2-Qualcomm-qcs6490-Add-DMI-match-for-Radxa-Dragon.patch"
    "0004-ucm2-Qualcomm-add-Radxa-Dragon-Q8B.patch"
    "0005-ucm2-Qualcomm-add-Radxa-CM-Q64.patch"
    "0006-ucm2-codecs-wcd938x-change-CLS_AB_LOHIFI-to-CLS_AB_H.patch"
    "0007-ucm2-Qualcomm-radxa-dragon-q8b-Use-the-CLS_AB_HIFI-w.patch"
    "0008-ucm2-Qualcomm-add-Radxa-DragonBay-4-Pro.patch"
    "0009-ucm2-Qualcomm-add-Radxa-DragonStation-6.patch"
)
b2sums=(
    'SKIP'
    'd651213507685eb8040bd4705e1eaea8fa4a723fa6474958cdac20534a436915ebd7bfed5b61594302ea5798b6af9583e8d71b01b7114aa8abf376b193a15e4d'
    '15e248c42d1e73535c1fe812948196cb8fa62b43c0a68e6bf44cef66cdc318f7a057675c128cc7cff0b9aed53bb5ee1c6d8ce648be4a95946982680cc026e8ae'
    'bcc68bf689f08e726d993edf9f2cf3cfe5bcb2508bc704a8ef131e413aa3441bcbdf7a8c637790a161b156ca75fdea93d82cbe1529d07b17c30c59ff3e29546f'
    '15468abefab13c851fa54edd28a219cc4ff1cdd1221263c0e876865b6521f7c1ca33b4ece29a99dd185c98c41b33f2db0119ef461141fa41f3a495aa89c888de'
    'b006ed22c2ff665a8115a10fe2f16b9e394f6a5d0d2ed8e15330bb28ab77caadd4c8351ff50bb52436d25b2026a5a0f376baf541685046206aa3c27ae8c344bb'
    'f9706129b35b1aeec78a93915b6df8dfa29435e696178dfd629ee3e78c1ed499e29996fba5184c48203a97e9ce2c4ac5feabbe7332868fefe62b58f21d054841'
    '464daf45e51c7592b8acf55c3fe68dfee0d569278eb9883e6fe9c84ae077d0db3428de204d713fc288e838fec7bd2e7f91ba0655a11b215dfe9ba84a7834dab4'
    '3d6e2c8ab4055c9ad203b0575aaacb568fcd800bae9975b14624e512c68abe0506bdcc73c2f4dc2c28f297f9a5675b4142fd783ac536435b5da91a9656e288dd'
)

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
        patch --batch --directory="${pkgbasename}" --forward --strip=2 --input="${srcdir}/${_patchfile}"
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
