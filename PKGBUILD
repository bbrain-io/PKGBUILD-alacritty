#!/bin/bash
# Maintainer: Jonathan SCHERRER <jonathan@bbrain.io>
# shellcheck disable=SC2034

pkgname=alacritty
pkgver=0.11.0
pkgrel=1
pkgdesc='A cross-platform, OpenGL terminal emulator.'
arch=('amd64')
depends=('libc6')
makedepends=(
    'cargo'
    'cmake'
    'desktop-file-utils'
    'git'
    'pkg-config'
    'libfontconfig1-dev'
    'libfreetype6-dev'
    'libxcb-xfixes0-dev'
    'libxkbcommon-dev'
    'libegl1-mesa-dev'
    'python3'
    'rustc'
)
checkdepends=('fonts-dejavu')
license=('Apache-2.0')
url='https://github.com/alacritty/alacritty'

source=("git+${url}.git#tag=v${pkgver}")
sha256sums=('SKIP')

build() {
    cd "${pkgname}/" || exit 1
    env CARGO_INCREMENTAL=0 cargo build --release --locked
}

check() {
    cd "${pkgname}/" || exit 1
    env CARGO_INCREMENTAL=0 cargo test --release
}

package() {
    cd "${pkgname}/" || exit 1

    # needed when using nvidia drivers
    cat <<'EOF' >>alacritty.sh
#!/usr/bin/env bash
__EGL_VENDOR_LIBRARY_FILENAMES=/usr/share/glvnd/egl_vendor.d/50_mesa.json /usr/bin/alacritty-bin "$@"
EOF

    # shellcheck disable=SC2154
    desktop-file-install -m 644 --dir "${pkgdir}/usr/share/applications/" "extra/linux/Alacritty.desktop"

    install -D -m755 "target/release/alacritty" "${pkgdir}/usr/bin/alacritty-bin"
    install -D -m755 "alacritty.sh" "${pkgdir}/usr/bin/alacritty"
    install -D -m644 "extra/alacritty.man" "${pkgdir}/usr/share/man/man1/alacritty.1"
    install -D -m644 "extra/linux/org.alacritty.Alacritty.appdata.xml" "${pkgdir}/usr/share/appdata/org.alacritty.Alacritty.appdata.xml"
    install -D -m644 "alacritty.yml" "${pkgdir}/usr/share/doc/alacritty/example/alacritty.yml"
    install -D -m644 "extra/completions/alacritty.bash" "${pkgdir}/usr/share/bash-completion/completions/alacritty"
    install -D -m644 "extra/completions/_alacritty" "${pkgdir}/usr/share/zsh/site-functions/_alacritty"
    install -D -m644 "extra/completions/alacritty.fish" "${pkgdir}/usr/share/fish/vendor_completions.d/alacritty.fish"
    install -D -m644 "extra/logo/compat/alacritty-term.png" "${pkgdir}/usr/share/pixmaps/Alacritty.png"
    install -D -m644 "extra/logo/alacritty-term.svg" "${pkgdir}/usr/share/pixmaps/Alacritty.svg"
}

# vim: set sw=4 expandtab:
