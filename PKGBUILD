# Maintainer: Selimaj Dev <https://github.com/selimaj-dev>
# Contributor: Radu Ursache <https://github.com/rursache>

pkgname=linear-linux-bin
pkgver=0.2.5
pkgrel=1
pkgdesc="Unofficial Electron-based Linux desktop client for Linear (linear.app)"
arch=('x86_64' 'aarch64')
url="https://github.com/selimaj-dev/linear-linux"
license=('Apache-2.0')
depends=('gtk3' 'nss' 'alsa-lib')
provides=('linear-linux')
conflicts=('linear-linux')
options=('!strip' '!debug')

_appimage_x86_64="linear-linux-${pkgver}-x86_64.AppImage"
_appimage_aarch64="linear-linux-${pkgver}-arm64.AppImage"

source_x86_64=("${_appimage_x86_64}::${url}/releases/download/v${pkgver}/${_appimage_x86_64}")
source_aarch64=("${_appimage_aarch64}::${url}/releases/download/v${pkgver}/${_appimage_aarch64}")

sha256sums_x86_64=('34199f5f2ae6b4171c38b2f3062c1b902c80f081b1b58f804088005717eb6a79')
sha256sums_aarch64=('9cd0e8edcab4e5ab34aa811b775524386facf0fcc56719e8bdca8bb20d754655')

prepare() {
    if [[ "${CARCH}" == "x86_64" ]]; then
        _appimage="${_appimage_x86_64}"
    else
        _appimage="${_appimage_aarch64}"
    fi

    chmod +x "${_appimage}"
    "./${_appimage}" --appimage-extract >/dev/null 2>&1
}

package() {
    cd "${srcdir}/squashfs-root"

    # Install the full app to /opt
    install -d "${pkgdir}/opt/linear-linux"
    cp -a . "${pkgdir}/opt/linear-linux/"

    # Fix permissions
    find "${pkgdir}/opt/linear-linux" -type d -exec chmod 755 {} +
    find "${pkgdir}/opt/linear-linux" -type f -exec chmod 644 {} +
    chmod 755 "${pkgdir}/opt/linear-linux/linear-linux"
    chmod 755 "${pkgdir}/opt/linear-linux/chrome-sandbox"
    chmod 4755 "${pkgdir}/opt/linear-linux/chrome-sandbox"

    # Install icons
    install -Dm644 usr/share/icons/hicolor/1024x1024/apps/linear-linux.png \
        "${pkgdir}/usr/share/icons/hicolor/1024x1024/apps/linear-linux.png"
    install -Dm644 resources/assets/linear-icon.svg \
        "${pkgdir}/usr/share/icons/hicolor/scalable/apps/linear-linux.svg"

    # Install .desktop file
    install -Dm644 /dev/stdin "${pkgdir}/usr/share/applications/linear-linux.desktop" <<EOF
[Desktop Entry]
Type=Application
Name=Linear
Comment=Linux support for linear.app
Exec=linear-linux %U
Icon=linear-linux
Terminal=false
Categories=Office;ProjectManagement;
StartupWMClass=Linear
EOF

    # Install launcher symlink
    install -d "${pkgdir}/usr/bin"
    ln -s /opt/linear-linux/linear-linux "${pkgdir}/usr/bin/linear-linux"
}
