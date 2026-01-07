pkgname=edb-debugger-bin
pkgver=1
pkgrel=1
pkgdesc="EDB (Evan's Debugger) is a cross platform AArch32/x86/x86-64 debugger, inspired by Ollydbg."
url='http://www.codef00.com/projects#debugger'
arch=(x86_64)
license=(GPL2)
options=(!debug !lto !strip)
depends=(qt5-xmlpatterns qt5-svg capstone graphviz)
makedepends=(patchelf)

prepare() {
  flatpak remote-add --user --if-not-exists flathub 'https://dl.flathub.org/repo/flathub.flatpakrepo'
  flatpak install -y --noninteractive --no-deps flathub 'io.github.eteran.edb-debugger'
}

package() {
  mkdir -p "$pkgdir/usr/"{bin,lib/edb,share/applications,share/man/man1,share/pixmaps}
  pushd "$HOME/.local/share/flatpak/app/io.github.eteran.edb-debugger/current/active/files/"
  patchelf --replace-needed libcgraph.so.6 libcgraph.so bin/edb
  patchelf --replace-needed libgvc.so.6 libgvc.so bin/edb
  echo -ne $(od -An -tx1 -v 'bin/edb' | tr -d '\n' | sed -e 's/00 2f 61 70 70 2f/00 2f 75 73 72 2f/g' -e 's/ /\\x/g') > 'bin/edb.patched'
  chmod --reference=bin/edb bin/edb.patched
  cp 'bin/edb.patched' "$pkgdir/usr/bin/edb"
  cp 'lib/edb/'*.so "$pkgdir/usr/lib/edb/"
  cp 'share/applications/io.github.eteran.edb-debugger.desktop' "$pkgdir/usr/share/applications/edb.desktop"
  cp 'share/man/man1/edb.1' "$pkgdir/usr/share/man/man1/"
  cp 'share/pixmaps/edb.png' "$pkgdir/usr/share/pixmaps/"
  popd
  sed -i 's/io\.github\.eteran\.edb-debugger/edb/' "$pkgdir/usr/share/applications/edb.desktop"
  sed -i '/X-Flatpak-RenamedFrom/d' "$pkgdir/usr/share/applications/edb.desktop"
}

pkgver() {
  local ver=$(flatpak list | awk '$1=="edb" {print $3}')
  printf '%s' "$ver"
}
