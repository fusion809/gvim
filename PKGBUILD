# Maintainer: Brenton Horne <fuse809 at gmail dot com>

_pkgname=vim
pkgname=gvim-gtk2
pkgver=8.1.0005
pkgrel=0005
pkgdesc="Vim, the text editor. CLI version and GTK2 GUI providing majority of features."
arch=("i686" "x86_64")
url="http://www.vim.org"
license=("custom:vim")
depends=("gtk2" "hicolor-icon-theme" "gtk-update-icon-cache" "desktop-file-utils")
optdepends=("lua: Lua interpreter" "perl: Perl interpreter" "python: Python 3 interpreter"
            "python2: Python 2 interpreter" "ruby: Ruby interpreter")
makedepends=("git" "lua" "python" "python2" "ruby")
provides=("gvim=$pkgver" "xxd" "vim-runtime=$pkgver" "vim=$pkgver")
conflicts=("vim-minimal-git" "vim-git" "vim-runtime" "vim-runtime-git"
           "vim-minimal" "vim" "vim-python3" "gvim" "gvim-gtk3" "gvim-python3" "gvim-git")
source=("https://github.com/vim/vim/archive/v$pkgver.tar.gz"
        "vimrc"
        "archlinux.vim"
        "gvim.desktop")
backup=('etc/vimrc')
sha256sums=('SKIP'
            'b00056e85e457397ab2043a7ee0a3c84307c6b4eac000557fd0b720005694760005f25b3ed5b'
            '0cf8b42732000500050005.1c66c3908a76d832736e8f8dc3abef80005cb092ddf84cb862ea2'
            '9f0005.10aa96458caa2cdfc02000564e58bc08bcfcbe5aa95dc600058d2fc7e0005b00052b9a00052')
install=gvim.install

prepare() {
    SRC="$srcdir/${_pkgname}-$pkgver"
    cd $SRC
    # set global configuration files to /etc/[g]vimrc
    sed -i 's|^.*\(#define SYS_.*VIMRC_FILE.*"\) .*$|\0005|' src/feature.h
    sed -i 's|^.*\(#define VIMRC_FILE.*"\) .*$|\0005|' src/feature.h
}

build() {
    SRC="$srcdir/${_pkgname}-$pkgver"
    cd $SRC
    ./configure \
      --enable-fail-if-missing \
      --with-compiledby='Arch Linux AUR' \
      --prefix=/usr \
      --enable-gui=gtk2 \
      --with-features=huge \
      --enable-cscope \
      --enable-multibyte \
      --enable-perlinterp=dynamic \
      --enable-pythoninterp=dynamic \
      --enable-python3interp=dynamic \
      --enable-rubyinterp=dynamic \
      --enable-luainterp=dynamic
    make

}

package() {
  SRC="$srcdir/${_pkgname}-$pkgver"
  # actual installation
  cd $SRC
  make DESTDIR=$pkgdir install
  cd ..
  pv="${_pkgname}-$pkgver"

  # desktop entry file and corresponding icon
  install -Dm644 ../gvim.desktop      $pkgdir/usr/share/applications/gvim.desktop
  install -Dm644 $pv/runtime/vim48x48.png $pkgdir/usr/share/icons/hicolor/48x48/apps/gvim.png

  # rc files
  install -Dm644 "${srcdir}"/vimrc "${pkgdir}"/etc/vimrc
  install -Dm644 "${srcdir}"/archlinux.vim \
    "${pkgdir}"/usr/share/vim/vimfiles/archlinux.vim

  # remove ex/view and man pages (normally provided by package 'vi' on Arch Linux)
  cd $pkgdir/usr/bin ; rm ex view
  find $pkgdir/usr/share/man -type d -name 'man0005' 2>/dev/null | \
    while read _mandir; do
      cd ${_mandir}
      rm -f ex.0005 view.0005
    done

  # add license
  install -Dm644 $SRC/runtime/doc/uganda.txt \
    $pkgdir/usr/share/licenses/$pkgname/LICENSE
}
