pkgname=deemixplus
pkgver=0.5.1.r378d15b
pkgrel=1
pkgdesc="Deemix+ desktop app with playback and download support"
arch=('x86_64')
url="https://github.com/ALi3naTEd0/deemix"
license=('GPL-3.0-only')
depends=(
	'alsa-lib'
	'at-spi2-core'
	'cairo'
	'dbus'
	'expat'
	'fontconfig'
	'gcc-libs'
	'gdk-pixbuf2'
	'glib2'
	'gtk3'
	'libdrm'
	'libx11'
	'libxcb'
	'libxcomposite'
	'libxdamage'
	'libxext'
	'libxfixes'
	'libxkbcommon'
	'libxrandr'
	'libxrender'
	'libxshmfence'
	'libxss'
	'mesa'
	'nspr'
	'nss'
	'pango'
	'systemd-libs'
	'wayland'
	'xdg-utils'
	'zlib'
)
makedepends=('git' 'nodejs>=24' 'pnpm')
_commit=378d15b33568ab223bd03bc3df7441d680dd8337
source=("git+https://github.com/ALi3naTEd0/deemix.git#commit=${_commit}")
sha256sums=('SKIP')

prepare() {
	cd "$srcdir/deemix"
	pnpm install --frozen-lockfile
}

build() {
	cd "$srcdir/deemix"
	pnpm build
	pnpm --filter deemix-gui exec electron-forge package --platform=linux --arch=x64
}

package() {
	local appdir="$srcdir/deemix/packages/gui/out/Deemix+-linux-x64"

	[[ -d "$appdir" ]] || error "Electron bundle not found: $appdir"

	install -d "$pkgdir/opt/deemixplus"
	cp -a "$appdir/." "$pkgdir/opt/deemixplus/"
	install -Dm644 "$srcdir/deemix/LICENSE.txt" \
		"$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
	install -Dm644 "$srcdir/deemix/packages/gui/build/64x64.png" \
		"$pkgdir/usr/share/pixmaps/deemixplus.png"

	install -d "$pkgdir/usr/bin"
	cat > "$pkgdir/usr/bin/deemixplus" <<'EOF'
#!/bin/sh
exec /opt/deemixplus/deemix-gui "$@"
EOF
	chmod 755 "$pkgdir/usr/bin/deemixplus"

	install -d "$pkgdir/usr/share/applications"
	cat > "$pkgdir/usr/share/applications/deemixplus.desktop" <<'EOF'
[Desktop Entry]
Name=Deemix+
Comment=Desktop audio player and download manager
Exec=deemixplus %U
Icon=deemixplus
Terminal=false
Type=Application
Categories=AudioVideo;Audio;Player;
EOF
}
