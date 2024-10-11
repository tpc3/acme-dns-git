# Maintainer: hayabusa2yk <me at h2yk dot dev>
# Contributor: Tony Finn <aurcomments at tonyfinn dot com>
# Contributor: lf <packages at lfcode dot ca>

pkgname=acme-dns-git
pkgver=r214.27e8251
pkgrel=1
pkgdesc="DNS server for ACME dns-01 challenges"
arch=('x86_64')
url="https://github.com/joohoi/acme-dns"
license=('MIT')
makedepends=('go')
conflicts=('acme-dns')
options=('!strip' '!emptydirs')
source=(
  'sysusers.conf'
  'tmpfiles.conf'
  'acme-dns.service'
  "${pkgname}::git+https://github.com/joohoi/acme-dns.git"
)
sha256sums=('edd9873e5d24736b9164467f7543dab42dc177d6c98b4ca6a572dfcbdd2202a4'
  'e8a8b3ffda44a3c346925ba4de197f1b6f139bfe52b9f77e6cb3f87677d3ad98'
  'dea89dea17e9f0eff9adf5af56a876ceb738d94f0281a6ece9ca841ec39cf6be'
  'SKIP')
backup=('etc/acme-dns/config.cfg')

pkgver() {
  cd "${pkgname}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "${pkgname}"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -mod=readonly -modcacherw"

  go build -v
}

package() {
  install -Dm644 "acme-dns.service" "$pkgdir/usr/lib/systemd/system/acme-dns.service"
  install -Dm644 "sysusers.conf" "$pkgdir/usr/lib/sysusers.d/acme-dns.conf"
  install -Dm644 "tmpfiles.conf" "$pkgdir/usr/lib/tmpfiles.d/acme-dns.conf"

  cd "${pkgname}"

  install -Dm755 "acme-dns" "$pkgdir/usr/bin/acme-dns"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/acme-dns/LICENSE"
  install -Dm644 config.cfg "$pkgdir/etc/acme-dns/config.cfg"
}
