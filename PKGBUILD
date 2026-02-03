# Maintainer: Phil <phil@example.com>
pkgname=whisper-toggle
pkgver=1.0.0
pkgrel=1
pkgdesc="Single-keybinding speech-to-text for Linux — press once to record, press again to transcribe and paste"
arch=('any')
url="https://github.com/philcutter/whisper-toggle"
license=('MIT')
depends=(
    'bash'
    'sox'
    'curl'
    'jq'
    'libnotify'
    'libpulse'
)
optdepends=(
    'whisper.cpp: speech recognition engine (or build from source)'
    'xsel: clipboard support on X11'
    'xdotool: auto-paste on X11'
    'wl-clipboard: clipboard support on Wayland'
    'ydotool: auto-paste on Wayland'
    'pciutils: GPU detection in setup wizard'
)
install=whisper-toggle.install
source=()
sha256sums=()

package() {
    cd "$startdir"
    install -Dm755 bin/whisper-toggle "$pkgdir/usr/bin/whisper-toggle"
    install -Dm755 bin/whisper-toggle-setup "$pkgdir/usr/bin/whisper-toggle-setup"
    install -Dm644 config/whisper-toggle.conf "$pkgdir/usr/share/whisper-toggle/whisper-toggle.conf"
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
