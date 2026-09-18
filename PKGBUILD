### GIT - DO NOT COMMIT
###
###
###
pkgname=lemonade-server
pkgdesc="Lemonade: Local LLM Serving with GPU and NPU acceleration (Server)"
pkgver=2026.39.1
pkgrel=1
arch=('x86_64')
url='https://github.com/lemonade-sdk/lemonade/'
license=('Apache-2.0')
makedepends=('cmake' 'ninja' 'git' 'cli11' 'nlohmann-json' 'libdrm' 'nodejs' 'npm')
depends=('zstd' 'unzip' 'curl' 'mbedtls' 'libwebsockets')
provides=('lemonade-server')
backup=('etc/lemonade/conf.d/zz-secrets.conf')
_httplibver=0.56.0
_cores=8

source=(
"${pkgname}::git+https://github.com/lemonade-sdk/lemonade#branch=release-v2026.39"
# "${pkgname}::git+https://github.com/lemonade-sdk/lemonade#branch=superm1/fix-migration"
# "${pkgname}::git+https://github.com/lemonade-sdk/lemonade#branch=fix/example"
# "${pkgname}::git+https://github.com/lemonade-sdk/lemonade#commit=hash"
# "${pkgname}::git+https://github.com/sofiageo/lemonade#branch=fix/example"
"httplib-${_httplibver}.tar.gz::https://github.com/yhirose/cpp-httplib/archive/refs/tags/v${_httplibver}.tar.gz"
tmpfiles.conf
)

sha256sums=(  
'SKIP'
'SKIP'
'SKIP'
)

build() {
  local cmake_options=(
    -B build
    -G Ninja
    -S $pkgname
    -W no-dev
    -D FETCHCONTENT_FULLY_DISCONNECTED=ON
    -D FETCHCONTENT_SOURCE_DIR_HTTPLIB="${srcdir}/cpp-httplib-${_httplibver}"    
    -D CMAKE_BUILD_TYPE=None
    -D CMAKE_INSTALL_PREFIX=/usr
  )
  cmake "${cmake_options[@]}"
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -dm0755 "${pkgdir}"/var/lib/lemonade
  install -dm0755 "${pkgdir}"/etc/lemonade
  sed -i 's/^StateDirectoryMode=0750$/StateDirectoryMode=0755/' "${pkgdir}"/usr/lib/systemd/system/lemond.service
  sed -i 's/^u/u!/' "${pkgdir}"/usr/lib/sysusers.d/lemonade.conf
  install -vDm644 "${srcdir}/tmpfiles.conf" "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"
}
