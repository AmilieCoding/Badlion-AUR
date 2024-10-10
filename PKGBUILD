# Maintainer: Amilie - amiliefn@gmail.com
# Contributor: Wings-Fantasy -  hxshijie@outlook.com
_pkgname=badlion-client

pkgname="${_pkgname}"-appimage
pkgver=4.4.0
pkgrel=1
pkgdesc="Badlion Client for Minecraft"
arch=('x86_64')
url="https://www.badlion.net/"
license=('custom:Unlicense')
depends=('zlib' 'hicolor-icon-theme')
options=(!strip)
_appimage="${pkgname}-${pkgver}.AppImage"
source_x86_64=("${_appimage}::https://client-updates-cdn77.badlion.net/BadlionClient"
              )
noextract=("${_appimage}")
sha256sums_x86_64=('972bb4590b647af09db866f91f0e43efd09e9281d3422287be87e7a92e7b9955')

prepare() {
    chmod +x "${_appimage}"
    ./"${_appimage}" --appimage-extract
}

build() {
    # Adjust .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=AppRun|Exec=env DESKTOPINTEGRATION=false /usr/bin/${_pkgname}|"\
        "squashfs-root/BadlionClient.desktop"
    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {
    # AppImage
    install -Dm755 "${srcdir}/${_appimage}" "${pkgdir}/opt/${pkgname}/${pkgname}.AppImage"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/BadlionClient.desktop"\
            "${pkgdir}/usr/share/applications/BadlionClient.desktop"

    # Icon images
    install -dm755 "${pkgdir}/usr/share/"
    cp -a "${srcdir}/squashfs-root/usr/share/icons" "${pkgdir}/usr/share/"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}.AppImage" "${pkgdir}/usr/bin/${_pkgname}"

    # Symlink license
    install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}/"
    ln -s "/opt/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname"
}
