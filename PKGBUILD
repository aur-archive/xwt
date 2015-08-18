pkgname=xwt
pkgver=20130308
pkgrel=1
pkgdesc="Xwt is a new .NET framework for creating desktop applications that run on multiple platforms from the same codebase."
arch=(any)
license=("custom:Xamarin")
depends=(gtk-sharp-2)
makedepends=(mono git)
options=(!strip)
url="https://github.com/mono/xwt"
_gitroot="https://github.com/mono/xwt.git"
_gitname="xwt"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]
  then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."
  rm -rf "$srcdir/$_gitname-build"
  cp -r "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  xbuild Xwt.sln /p:Configuration=Linux-Debug
}

package() {
  cd "$srcdir/$_gitname-build/Xwt.Gtk/bin/Debug"
  find . -name '*.dll' -o -name '*.mdb' -o -name '*.config' |
    xargs -rtl1 -I {} install -Dm644 {} "$pkgdir/usr/lib/xwt/"{}
  find "$pkgdir/usr/lib/xwt" -name '*.dll' | xargs -rtl1 -I {} gacutil -i {} -root "$pkgdir/usr/lib"
}
