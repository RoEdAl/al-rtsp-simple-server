# Maintainer: Edmunt Pienkowsky

# Based on http://aur.archlinux.org/packages/rtsp-simple-server-git

pkgname=rtsp-simple-server
pkgver=0.17.16
pkgrel=1
pkgdesc="Ready-to-use RTSP server and RTSP proxy that allows to read, publish and proxy video and audio streams"
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'armv6h')
url="http://github.com/aler9/rtsp-simple-server"
license=('MIT')
depends=('glibc')
backup=(
	"etc/${pkgname}/${pkgname}.yml"
)
makedepends=('go' 'git')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=("${pkgname}-${pkgver}.tar.gz::http://github.com/aler9/${pkgname}/archive/v${pkgver}.tar.gz"
        'rtsp-simple-server.service')
md5sums=('441c5c949cb9b5e0588ec6df78620e91'
         '81b313c4715f2ec997c64aa47f75e444')

prepare() {
	msg2 'Cache setup'
	local GOCACHE="$srcdir/cache/go"
	local GOTMP="$srcdir/tmp/go"
	mkdir -p "$GOCACHE"
	mkdir -p "$GOTMP"

	msg2 'GOPATH setup'
	local GOPATH="$srcdir/gopath"

	cd "$srcdir/${pkgname}-${pkgver}"
	mkdir -p build
}

build() {
	local GOCACHE="$srcdir/tmp/cache"
	local GOTMP="$srcdir/tmp/go"
	local GOPATH="$srcdir/gopath"

	local -r ENV_ARGS=(
		"PATH=$PATH:$GOPATH/bin"
		TMPDIR="$GOTMP"
		"GOCACHE=$GOCACHE"
		"GOPATH=$GOPATH"
		"GOTMPDIR=$GOTMP"
		"CGO_CPPFLAGS=${CPPFLAGS}"
		"CGO_CFLAGS=${CFLAGS}"
		"CGO_CXXFLAGS=${CXXFLAGS}"
		'GOFLAGS=-buildmode=pie -modcacherw -trimpath'
	)

	cd "$srcdir/${pkgname}-${pkgver}"
	env "${ENV_ARGS[@]}" go build -o build .
}

package() {
	install -Dm644 rtsp-simple-server.service "$pkgdir"/usr/lib/systemd/system/rtsp-simple-server.service

	cd "$srcdir/${pkgname}-${pkgver}"
	install -Dm755 build/${pkgname} "$pkgdir"/usr/bin/${pkgname}
	install -Dm644 rtsp-simple-server.yml "$pkgdir/etc/${pkgname}/${pkgname}.yml"
}
