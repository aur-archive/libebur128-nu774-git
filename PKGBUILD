# Maintainer: Robert Mackanics <schnoopay@gmail.com>
# Contributor: Vasil Yonkov <bustervill at gmail dot com>
pkgname=libebur128-nu774-git
pkgver=20121229
pkgrel=2
pkgdesc="A library implementing the EBU R128 loudness standard, nu774 fork."
arch=('x86_64')
url="https://github.com/nu774/libebur128"
license=('MIT')
makedepends=('git' 'cmake')
optdepends=('gtk2: scanner dependency'
            'qt: scanner dependency'
            'taglib: scanner dependency'
            'libsndfile: SNDFILE input module support'
            'mpg123: MPG123 input module support'
            'libmpcdec: MPCDEC input module support'
            'ffmpeg: FFMPEG input module support'
            'gstreamer0.10: GStreamer  input module support'
            'opusfile: opus tagging support')
conflicts=('libebur128-git')

_gitroot="git://github.com/nu774/libebur128.git"
_gitname="libebur128"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server..."
  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone --depth=1 $_gitroot $_gitname
  fi
  git submodule init
  git submodule update
  msg "GIT checkout done or server timeout"

  rm -rf "$srcdir/$_gitname-build"
  mkdir "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname" && ls -A | grep -v .git | xargs -d '\n' cp -r -t ../$_gitname-build
  cd "$srcdir/$_gitname-build"

  mkdir build
  cd build

  cmake -DCMAKE_INSTALL_LIBDIR:path=lib -DCMAKE_INSTALL_PREFIX=/usr ..
  make
}

package() {
  cd "$srcdir/$_gitname-build/build"
  make DESTDIR="$pkgdir/" install

  install -d -m755 ${pkgdir}/usr/share/licenses/${pkgname}/
  ln -sf /usr/share/doc/COPYING ${pkgdir}/usr/share/licenses/${pkgname}/
}

# vim:set ts=2 sw=2 et:
