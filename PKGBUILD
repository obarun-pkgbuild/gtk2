# Maintainer : Danial (bytn) Spruce <bit@teknik.io>
# Contributor : Eric Vidal <eric@obarun.org>
# based on the original https://git.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/gtk2
# 						$Id: PKGBUILD 272006 2016-07-19 14:28:51Z jgc $
# 						Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=gtk2
pkgver=2.24.32
pkgrel=2
pkgdesc="GObject-based multi-platform GUI toolkit (legacy)"
arch=(x86_64)
url="https://www.gtk.org/"
depends=('atk' 'pango' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage'
         'shared-mime-info' 'cairo' 'gtk-update-icon-cache' 'librsvg' 'desktop-file-utils')
makedepends=(gobject-introspection python2 git gtk-doc)
optdepends=('gnome-themes-standard: Default widget theme'
            'adwaita-icon-theme: Default icon theme')
license=(LGPL)
install=gtk2.install
_commit=ed7d3e25f8b6debae6ccc8b50d1329155338cab8 # tags/2.24.32^0
#Fixme: If arch breaks gtk2, Replace commit with tree from upstream
source=("git+https://git.gnome.org/browse/gtk+#commit=$_commit"
        gtkrc
        gtk-query-immodules-2.0.hook
        xid-collision-debug.patch)
sha256sums=('SKIP'
            'bc968e3e4f57e818430130338e5f85a5025e21d7e31a3293b8f5a0e58362b805'
            '9656a1efc798da1ac2dae94e921ed0f72719bd52d4d0138f305b993f778f7758'
            'd758bb93e59df15a4ea7732cf984d1c3c19dff67c94b957575efea132b8fe558')
 
validpgpkeys=('EE62628C5670B472C1529563D4CA511F0375F9B2') # Danial Spruce

pkgver() {
  cd gtk+
  git describe --tags | sed 's/-/+/g'
}

prepare() {
    cd gtk+
    patch -Np1 -i ../xid-collision-debug.patch
    sed -i '1s/python$/&2/' gtk/gtk-builder-convert
    NOCONFIGURE=1 ./autogen.sh
}

build() {
    cd gtk+

    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --with-xinput=yes \
        --enable-gtk-doc \
        --disable-cups

    # https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

    make
}

package() {
    cd gtk+
    make DESTDIR="$pkgdir" install

    install -Dm644 ../gtkrc "$pkgdir/usr/share/gtk-2.0/gtkrc"
    install -Dm644 ../gtk-query-immodules-2.0.hook "$pkgdir/usr/share/libalpm/hooks/gtk-query-immodules-2.0.hook"

    rm "$pkgdir/usr/bin/gtk-update-icon-cache"
}

# vim:set et sw=4:
