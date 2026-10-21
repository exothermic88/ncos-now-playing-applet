# Maintainer: nic <nicolasciampa2001@gmail.com>
# Personal-repo PKGBUILD: builds the upstream "Now Playing" COSMIC media applet
# from a pinned release tag. Bump pkgver + updpkgsums to update.
pkgname=ncos-now-playing-applet
_upstream=cosmic-media-now-playing-applet
_appid=com.github.cosmic_media_now_playing_applet
pkgver=0.26.0
pkgrel=1
pkgdesc="COSMIC panel applet showing the currently playing media track via MPRIS (Now Playing)"
arch=('x86_64' 'aarch64')
url="https://github.com/stldave314/cosmic-media-now-playing-applet"
license=('GPL-3.0-only')
depends=('libxkbcommon' 'wayland') # wayland: libwayland-client is dlopen'd at runtime
makedepends=('cargo' 'just' 'git') # git: libcosmic is a cargo git dependency
provides=('cosmic-media-now-playing-applet')
conflicts=('cosmic-media-now-playing-applet')
options=('!lto') # cargo already does its own LTO; avoid makepkg injecting -flto
source=("$_upstream-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('3cced8e54e9fbef6c59bd1a68b8c7d14e16f150be0e8f036b7aab22b1668d7c6')

prepare() {
    cd "$_upstream-$pkgver"
    cargo fetch --locked
}

build() {
    cd "$_upstream-$pkgver"
    # release-build: upstream feature that forces diagnostic logging off at
    # compile time; their own packaging targets (deb/rpm/flatpak) enable it.
    just build-release --offline --features release-build
}

package() {
    cd "$_upstream-$pkgver"
    install -Dm755 "target/release/$_upstream" "$pkgdir/usr/bin/$_upstream"
    install -Dm644 resources/app.desktop \
        "$pkgdir/usr/share/applications/$_appid.desktop"
    # Mirror upstream install.sh stage_metainfo: the newest <release> entry in
    # the source tree is stale, so set it to the packaged version and today's
    # date so software centers see the right version.
    sed -E "s|<release version=\"[^\"]*\" date=\"[^\"]*\"/>|<release version=\"$pkgver\" date=\"$(date +%Y-%m-%d)\"/>|" \
        resources/app.metainfo.xml > app.metainfo.xml
    install -Dm644 app.metainfo.xml \
        "$pkgdir/usr/share/metainfo/$_appid.metainfo.xml"
    install -Dm644 resources/icon.svg \
        "$pkgdir/usr/share/icons/hicolor/scalable/apps/$_appid.svg"
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
