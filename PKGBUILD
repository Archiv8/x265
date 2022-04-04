#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# ToDo: Add files: User documentation
# ToDo: Add files: Tooling
# FixMe: Namcap warnings and errors

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>


pkgname=x265
pkgver=3.5
pkgrel=1
pkgdesc='Open Source H265/HEVC video encoder'
arch=(x86_64)
url="https://bitbucket.org/multicoreware/x265_git"
license=("GPL")
depends=("gcc-libs")
makedepends=(
  "cmake"
  "git"
  "nasm"
  "ninja"
)
provides=("libx265.so")
_tag=f0c1022b6be121a753ff02853fbe33da71988656
source=("git+https://bitbucket.org/multicoreware/x265_git#tag=${_tag}")
sha256sums=("SKIP")

pkgver() {
  cd x265_git

  git describe --tags
}

build() {
  cmake -S x265_git/source -B build-12 -G Ninja \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DHIGH_BIT_DEPTH=TRUE \
    -DMAIN12=TRUE \
    -DEXPORT_C_API=FALSE \
    -DENABLE_CLI=FALSE \
    -DENABLE_SHARED=FALSE \
    -Wno-dev
  ninja -C build-12

  cmake -S x265_git/source -B build-10 -G Ninja \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DHIGH_BIT_DEPTH=TRUE \
    -DEXPORT_C_API=FALSE \
    -DENABLE_CLI=FALSE \
    -DENABLE_SHARED=FALSE \
    -Wno-dev
  ninja -C build-10

  cmake -S x265_git/source -B build -G Ninja \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DENABLE_SHARED=TRUE \
    -DENABLE_HDR10_PLUS=TRUE \
    -DEXTRA_LIB='x265_main10.a;x265_main12.a' \
    -DEXTRA_LINK_FLAGS='-L .' \
    -DLINKED_10BIT=TRUE \
    -DLINKED_12BIT=TRUE \
    -Wno-dev
  ln -s ../build-10/libx265.a build/libx265_main10.a
  ln -s ../build-12/libx265.a build/libx265_main12.a
  ninja -C build
}

package() {
  DESTDIR="${pkgdir}" ninja -C build install
}

# vim: ts=2 sw=2 et:
