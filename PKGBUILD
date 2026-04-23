# SPDX-License-Identifier: AGPL-3.0

#    -----------------------------------------------------
#    Copyright © 2022, 2023, 2024, 2025, 2026
#                Pellegrino Prevete
#
#    All rights reserved
#    -----------------------------------------------------
#
#    This program is free software: you can redistribute
#    it and/or modify it under the terms of the
#    GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    This program is distributed in the hope that it
#    will be useful, but WITHOUT ANY WARRANTY;
#    without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU Affero General Public License for
#    more details.
#
#    You should have received a copy of the
#    GNU Affero General Public License
#    along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>

# shellcheck disable=SC2034
os="$(
  uname \
    -o)"
if [[ ! -v "_libc" ]]; then
  if [[ "${_os}" == "Android" ]]; then
    _libc="ndk-sysroot"
  elif [[ "${_os}" == "GNU/Linux" ]]; then
    _libc="glibc"
  elif [[ "${_os}" == "MSys" ]]; then
    _libc="msys2-w32api-runtime"
  else
    _libc="glibc"
  fi
fi
if [[ ! -v "_compiler" ]]; then
  if [[ "${_os}" == "Android" ]]; then
    _compiler="clang"
  elif [[ "${_os}" == "GNU/Linux" ]]; then
    _compiler="gcc"
  elif [[ "${_os}" == "MSys" ]]; then
    _compiler="gcc"
  else
    _compiler="gcc"
  fi
fi
if [[ ! -v "_git" ]]; then
  _git="true"
fi
if [[ ! -v "_git_service" ]]; then
  _git_service="github"
fi
if [[ ! -v "_ns" ]]; then
  _ns="ps2dev"
fi
target=dvp
_platform="ps2"
_bu=binutils
_pkg=${_bu}-gdb
_base="toolchain"
pkgbase="${target}-${_bu}"
pkgname=(
  "${pkgbase}"
)
pkgver="2.14"
_pkgver="v${pkgver}"
_commit="9cca5c1781d1a03b9b3b61a3e5270cdb9c69295e"
pkgrel=1
_pkgdesc=(
  "A set of programs to assemble"
  "and manipulate binary and object files "
  "for the Sony PlayStation® 2 videogame"
  "system (binutils, ${target})."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'aarch64'
  'arm'
  'armv7l'
  'armv8l'
  'i686'
  'mips'
  'pentium4'
  'powerpc'
  'x86_64'
)
license=(
  'BSD'
)
depends=(
)
makedepends=(
  "${_compiler}"
  "make"
)
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ ! -v "_http" ]]; then
  _http="https://${_git_service}.com"
fi
_url="${_http}/${_ns}"
_local="ssh://git@127.0.0.1:/home/git"
url="${_github}/${_ns}/${_pkg}"
_tag_name="commit"
if [[ "${_tag_name}" == "branch" ]]; then
  _tag="${_pkgver}"
elif [[ "${_tag_name}" == "commit" ]]; then
  _tag="${_commit}"
fi
_tarname="${_pkg}-${_tag}"
if [[ "${_git}" == "true" ]]; then
  _src="${_tarname}::git+${_url}#${_tag_name}=${_commit}"
  _sum="SKIP"
fi
source=(
  "${_src}"
)
# source=(
#  "${pkgname}::git+${_local}/${_platform}-${_pkg}#branch=${_branch}"
# )
sha256sums=(
  "${_sum}"
)

_osver="$(
  uname)"
_n_cpu=$(
  getconf \
    _NPROCESSORS_ONLN)
# _make_opts=(
#   -j
#     "${_n_cpu}"
# )

# shellcheck disable=SC2154
build() {
  local \
    _target \
    _configure_opts=() \
    _build_opts=() \
    _make_opts=() \
    _cflags=() \
    _ldflags=()
  _cflags+=(
    -D_FORTIFY_SOURCE=0
    -O2
    -Wno-implicit-function-declaration
  )
  _ldflags+=(
    ${LDFLAGS}
    -s
  )
  _build_opts=(
    ${_make_opts[@]}
    CFLAGS="${_cflags[*]}"
    CPPFLAGS="${_cflags[*]}"
    LDFLAGS="${_ldflags[*]}"
  )
  export \
    CFLAGS=""
  cd \
    "${srcdir}/${_tarname}"
  for _target in "${target}"; do
    rm \
      -rf \
      "build-${_target}"
    mkdir \
      -p \
      "build-${_target}"
    cd \
      "build-${_target}"
    _configure_opts=(
      --prefix="/usr"
      --with-sysroot="/usr/${target}"
      --libdir="/usr/${target}/lib"
      --infodir="/usr/${target}/share/info"
      --mandir="/usr/${target}/man"
      --target="${target}"
      --disable-nls
      --disable-build-warnings
    )
    "../configure" \
      "${_configure_opts[@]}"
    make \
      "${_build_opts[@]}"
    cd \
      ".."
  done
}

# shellcheck disable=SC2154
package() {
  local \
    _target \
    _make_opts=()
  _make_opts+=(
    DESTDIR="${pkgdir}"
  )
  cd \
    "${srcdir}/${_tarname}"
  for _target in "${target}"; do
    cd \
      "build-${_target}"
    make \
      "${_make_opts[@]}" \
      install
    cd ..
  done
}
