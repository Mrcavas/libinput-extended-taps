# Maintainer: Mrcavas <sidorov.mrcavas@mail.ru>
pkgbase=libinput-extended-taps
pkgname=(
  libinput-extended-taps
  libinput-extended-taps-tools
)
pkgver=1.31.3
pkgrel=1
pkgdesc="Input device library (Patched for 4-finger taps and forced 1-finger drag)"
url="https://github.com/Mrcavas/libinput-extended-taps"
arch=(x86_64)
license=(MIT)
depends=(
  glibc libevdev libgcc libwacom lua54 mtdev systemd-libs
)
makedepends=(
  cairo check git glib2 gtk4 libx11 meson python python-libevdev
  python-pyudev python-yaml wayland wayland-protocols
)
source=(
  "git+https://gitlab.freedesktop.org/libinput/libinput.git#tag=$pkgver"
  "extended-taps.patch"
)
sha256sums=('8d804940190cb0c52fe33c06fb5f3e6c38d789a1429d5f9a1deae175e2ae18d4'
            'f0062c354b0a6ff548a4a923651a4f6f1d717a761db8118bd1032e51deb30257')

prepare() {
  cd libinput
  patch -Np1 -i "$srcdir/extended-taps.patch"
}

build() {
  local meson_options=(-D documentation=false)
  arch-meson libinput build "${meson_options[@]}"
  meson compile -C build
}

_pick() {
  local p="$1" f d; shift
  for f; do
    d="$srcdir/$p/${f#$pkgdir/}"
    mkdir -p "$(dirname "$d")"
    mv "$f" "$d"
    rmdir -p --ignore-fail-on-non-empty "$(dirname "$f")"
  done
}

package_libinput-extended-taps() {
  provides=(libinput libinput.so)
  conflicts=(libinput)
  optdepends=('libinput-tools: debug utilities')

  meson install -C build --destdir "$pkgdir"

  (
    cd "$pkgdir"
    _pick tools usr/bin
    _pick tools usr/lib/libinput
    _pick tools usr/share/man
    _pick tools usr/share/zsh
  )

  install -Dm644 libinput/COPYING -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_libinput-extended-taps-tools() {
  pkgdesc+=" (debug utilities)"
  provides=(libinput-tools)
  conflicts=(libinput-tools)
  depends=(
    cairo glib2 glibc gtk4 libevdev libgcc libinput libx11
    python python-libevdev python-pyudev python-yaml systemd-libs wayland
  )

  mv tools/* "$pkgdir"
  install -Dm644 libinput/COPYING -t "$pkgdir/usr/share/licenses/$pkgname"
}
