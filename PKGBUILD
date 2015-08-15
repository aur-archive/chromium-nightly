# Maintainer: Kenneth Endfinger <kaendfinger@gmail.com>
# Bugs - Please email me, I WILL take care of it
# This package will rock. I am a Chromium addict.

pkgname=chromium-nightly
pkgver=LATEST
pkgrel=2
pkgdesc="The open-source project behind Google Chrome (Web/HTTP/FTP Browser)"
arch=('i686' 'x86_64')
url="http://www.chromium.org/"
license=('custom:BSD')
depends=('alsa-lib' 'desktop-file-utils' 'flac' 'gconf' 'gtk2' 'nss' 'libxss' 'ttf-font' 'libgcrypt15')
optdepends=('ttf-hannom: Unicode Han and Nom (Chinese and Vietnamese) fonts'
            'otf-ipafont: Unicode Gothic/sans and Mincho/serif (Japanese) fonts [AUR]'
            'ttf-indic-otf: Unicode collection various (Indian) language fonts'
            'xdg-utils: for setting a default browser on desktop environments'
            'chromium-libpdf: Chrome PDF plugin integration [AUR]'
            'chromium-pepper-flash: Pepper Flash intergration [AUR]')
provides=('chromium' 'chromium-browser-bin')
conflicts=('chromium' 'chromium-browser-bin')
backup=('etc/chromium/default')
noextract=('chromium.1.gz')
install=chromium.install

_arch='Linux'
[ "$CARCH" = 'x86_64' ] && _arch='Linux_x64'
_pkgver=$(curl -s "http://commondatastorage.googleapis.com/chromium-browser-snapshots/$_arch/LAST_CHANGE")
PKGEXT='.pkg.tar'

source=("http://commondatastorage.googleapis.com/chromium-browser-snapshots/$_arch/$_pkgver/chrome-linux.zip"
        'chrome-wrapper.patch'
        'chromium.1.gz'
        'chromium.desktop'
        'chromium'
        'default'
        'LICENSE')
md5sums=('SKIP'
         'bddc7ab6aa35eaaa9691fca7ef2763df'
         '1774b5d79cfc67403fb336147a17e9a6'
         '7cc5d72aa4aaf528fb94e32c42a11493'
         'f5a19570988180388c7d9bfebbba6a86'
         '07056ee3cac7504b058084ad64f7cdcd'
         'b689219f39e74e0c0b19f10a1db1839d')

pkgver() {
  echo ${_pkgver}
}

package() {
  install -d "$pkgdir"/{etc/chromium,usr/{bin,lib,share/{applications,licenses/$pkgname,man/man1,pixmaps}}}
  mv chrome-linux "$pkgdir"/usr/lib/chromium

  msg2 "Patching script 'chrome-wrapper'..."
  cd "$pkgdir"/usr/lib/chromium
  patch -s -i "$srcdir"/chrome-wrapper.patch

  msg2 "Making it nice..."
  # Adjust permissions
  find "$pkgdir"/usr/lib/chromium -type d -exec chmod 755 {} ';'
  find "$pkgdir"/usr/lib/chromium -type f -exec chmod 644 {} ';'
  chmod 755 "$pkgdir"/usr/lib/chromium/chrome{,[_-]*}
  chmod 755 "$pkgdir"/usr/lib/chromium/nacl*
  chmod 755 "$pkgdir"/usr/lib/chromium/xdg-settings
  chmod 4755 "$pkgdir"/usr/lib/chromium/chrome_sandbox
  chown 1000 "$pkgdir"/usr/lib/chromium/chrome

  msg2 "Finishing up..."
  # Install default settings, wrapper script, desktop, license, manpages and icon
  cd "$srcdir"
  install -m644 default "$pkgdir"/etc/chromium/
  install -m755 chromium "$pkgdir"/usr/bin/
  install -m644 chromium.desktop "$pkgdir"/usr/share/applications/
  install -m644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/
  install -m644 chromium.1.gz "$pkgdir"/usr/share/man/man1/
  mv "$pkgdir"/usr/lib/chromium/chrome.1 "$pkgdir"/usr/share/man/man1/chromium.1
  mv "$pkgdir"/usr/lib/chromium/product_logo_48.png "$pkgdir"/usr/share/pixmaps/chromium.png

  msg2 "Symlinking 'libudev.so.1'..."
  # Symlink missing udev lib
  ln -fs /usr/lib/libudev.so.1 "$pkgdir"/usr/lib/libudev.so.0
}
