# Maintainer: 7Ji <pugokughin@gmail.com>

_desc="flippy's AArch64-focused fork aiming to increase usability. Prebuilt by ophub."
_pkgver_main=6.1.8
_pkgver_suffix=flippy-81+
_pkgver_uname="${_pkgver_main}-${_pkgver_suffix}"
_url_repo="https://github.com/ophub/kernel"
_url_raw="${_url_repo}/raw/main/pub/stable/${_pkgver_main}"
_pkgbase=linux-aarch64-flippy

pkgbase="${_pkgbase}-bin"
pkgname=(
  "${pkgbase}"
  "${pkgbase}-headers"
  "${pkgbase}-dtb-allwinner"
  "${pkgbase}-dtb-amlogic"
  "${pkgbase}-dtb-rockchip"
)
pkgver="${_pkgver_main}"
pkgrel=1
arch=('aarch64')
url="${_url_repo}"
license=('GPL2')
options=(!strip)
source=(
  "${_url_raw}/boot-${_pkgver_uname}.tar.gz"
  "${_url_raw}/dtb-allwinner-${_pkgver_uname}.tar.gz"
  "${_url_raw}/dtb-amlogic-${_pkgver_uname}.tar.gz"
  "${_url_raw}/dtb-rockchip-${_pkgver_uname}.tar.gz"
  "${_url_raw}/header-${_pkgver_uname}.tar.gz"
  "${_url_raw}/modules-${_pkgver_uname}.tar.gz"
)
noextract=("${source[@]##*/}")
sha256sums=(
  '05feb6dc6789952208723c5ee37b05078d544cb139d28de10f60cfa90f4be179'
  'acec35a813c02d2be1bd044f924c6f66535c401063da07a146c75e1792978e5f'
  'aa50ed2f7a09c4e9a0ac46a818f75d42b709c50fc59c5d18249a957710bca6c3'
  'd899b395dff1540f767a721eab7d1c33e33c6e322f831650739fcfe77fada250'
  'd18de0d355dd7d0c4762badab19bc4f699933d0749165a5633b21387924d4e36'
  '72d7a0980d62d665660ece11877ec9e591b6aff783a2dde76490aa1cb94155d2'
)

_dtb_common_pkg="${_pkgbase}-dtb"

package_linux-aarch64-flippy-bin() {
  pkgdesc="The Linux Kernel and module - ${_desc}"
  depends=(
    "${_dtb_common_pkg}"
    'coreutils'
    'initramfs'
    'kmod'
  )
  optdepends=(
    'uboot-legacy-initrd-hooks: to generate uboot legacy initrd images'
    'linux-firmware-amlogic-ophub: firmware for Amlogic SoCs'
    'wireless-regdb: to set the correct wireless channels of your country'
    "${_pkgbase}-dtb-allwinner: dtbs for Allwinner SoCs"
    "${_pkgbase}-dtb-amlogic: dtbs for Amlogic SoCs"
    "${_pkgbase}-dtb-rockchip: dtbs for Rockchip SoCs"
  )
  provides=(
    "${_pkgbase}=${pkgver}"
  )
  conflicts=(
    "${_pkgbase}"
  )
  backup=(
    "etc/mkinitcpio.d/${_pkgbase}.preset"
  )

  # Install modules
  install -d -m 755 "${pkgdir}/usr/lib/modules"
  tar -C "${pkgdir}/usr/lib/modules" -xvzf "${srcdir}/modules-${_pkgver_uname}.tar.gz"

  # Install pkgbase
  local _dir_module="${pkgdir}/usr/lib/modules/${_pkgver_uname}"
    echo "${_pkgbase}" | install -D -m 644 /dev/stdin "${_dir_module}/pkgbase"

  # Install kernel itself
  local _vmlinuz="vmlinuz-${_pkgver_uname}"
  tar -C "${_dir_module}" -xvzf "${srcdir}/boot-${_pkgver_uname}.tar.gz" "${_vmlinuz}"
  mv -v "${_dir_module}/${_vmlinuz}" "${_dir_module}/vmlinuz"

  # Remove non-standard folders
  rm -f "${pkgdir}/usr/lib/modules/${_pkgver_uname}/"{build,source}

  # install mkinitcpio preset file
  sed "s|%PKGBASE%|${_pkgbase}|g" ../linux.preset |
    install -Dm644 /dev/stdin "${pkgdir}/etc/mkinitcpio.d/${_pkgbase}.preset"
}

package_linux-aarch64-flippy-bin-headers() {
  pkgdesc="Header files and scripts for building modules for linux kernel - ${_desc}"
  provides=(
    "${_pkgbase}-headers=${pkgver}"
  )
  conflicts=(
    "${_pkgbase}-headers"
  )
  install -d -m 755 "${pkgdir}/usr/lib/modules/${_pkgver_uname}/build"
  tar -C "${pkgdir}/usr/lib/modules/${_pkgver_uname}/build" -xvzf "${srcdir}/header-${_pkgver_uname}.tar.gz"
  install -d -m 755 "${pkgdir}/usr/src"
  ln -sf "../lib/modules/${_pkgver_uname}/build" "${pkgdir}/usr/src/${_pkgbase}"
}

_dtb_common_provides="${_dtb_common_pkg}=${pkgver}"

package_linux-aarch64-flippy-bin-dtb-allwinner() {
  pkgdesc="DTB files for Allwinner SoCs for flippy's AArch64 kernel"
  provides=(
    "${_dtb_common_provides}"
    "${_pkgbase}-dtb-allwinner=${pkgver}"
  )
  conflicts=(
    "${_pkgbase}-dtb-allwinner"
  )
  install -d -m 755 "${pkgdir}/boot/dtbs/${_pkgbase}/allwinner"
  tar -C "${pkgdir}/boot/dtbs/${_pkgbase}/allwinner" -xvzf "${srcdir}/dtb-allwinner-${_pkgver_uname}.tar.gz"
}

package_linux-aarch64-flippy-bin-dtb-amlogic() {
  pkgdesc="DTB files for Amlogic SoCs for flippy's AArch64 kernel"
  provides=(
    "${_dtb_common_provides}"
    "${_pkgbase}-dtb-amlogic=${pkgver}"
  )
  conflicts=(
    "${_pkgbase}-dtb-amlogic"
  )
  install -d -m 755 "${pkgdir}/boot/dtbs/${_pkgbase}/amlogic"
  tar -C "${pkgdir}/boot/dtbs/${_pkgbase}/amlogic" -xvzf "${srcdir}/dtb-amlogic-${_pkgver_uname}.tar.gz"
}

package_linux-aarch64-flippy-bin-dtb-rockchip() {
  pkgdesc="DTB files for Rockchip SoCs for flippy's AArch64 kernel"
  provides=(
    "${_dtb_common_provides}"
    "${_pkgbase}-dtb-rockchip=${pkgver}"
  )
  conflicts=(
    "${_pkgbase}-dtb-rockchip"
  )
  install -d -m 755 "${pkgdir}/boot/dtbs/${_pkgbase}/rockchip"
  tar -C "${pkgdir}/boot/dtbs/${_pkgbase}/rockchip" -xvzf "${srcdir}/dtb-rockchip-${_pkgver_uname}.tar.gz"
}