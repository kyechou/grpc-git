# Copyright 2015 Aleksey Filippov

# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at

#     http://www.apache.org/licenses/LICENSE-2.0

# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Maintainer: Aleksey Filippov <sarum9in@gmail.com>
# Contributor: Victor Aurélio Santos <victoraur.santos@gmail.com>

pkgbase=grpc-git
pkgname=('grpc-git')
pkgver=1.18.0.r0.ga9cb80fef7
pkgrel=1
pkgdesc="A high performance, open source, general RPC framework that puts mobile and HTTP/2 first."
arch=('i686' 'x86_64')
url='https://grpc.io/'
license=('BSD')
depends=('c-ares' 'openssl-1.0' 'protobuf>=3')
provides=('grpc')
conflicts=('grpc')
source=("$pkgname"::'git+https://github.com/kyechou/grpc.git')
md5sums=('SKIP')

pkgver() {
  cd "$srcdir/$pkgname"
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir/$pkgname"

  # We are not interested in being strict here
  # regardless of warning type.
  sed -r 's|-Werror||g' -i Makefile
}

build() {
  cd "$srcdir/$pkgname"

  # gRPC is not compatible yet with openssl-1.1
  export PKG_CONFIG_PATH=/usr/lib/openssl-1.0/pkgconfig

  # Core
  # Avoid collision with yaourt's environment variable
  env --unset=BUILDDIR make $MAKEFLAGS prefix=/usr
}

_install_dir() (
  cd "$2"
  find . -type f -exec install "-Dm$1" '{}' "$pkgdir/$3/{}" ';'
  find . -type l -exec cp -a '{}' "$pkgdir/$3/{}" ';'
)

package() {
  cd "$srcdir/$pkgname"
  _install_dir 755 bins/opt usr/bin
  _install_dir 755 libs/opt usr/lib
  chmod 644 "$pkgdir/usr/lib/"*.a
  _install_dir 644 include usr/include
  install -Dm644 etc/roots.pem "$pkgdir/usr/share/grpc/roots.pem"
  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
