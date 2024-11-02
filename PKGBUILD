# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=sphinxcontrib-qthelp
_Pkg=sphinx-doc-qthelp
pkgname="${_py}-${_pkg}"
pkgver=2.0.0
pkgrel=1
pkgdesc='Sphinx extension which outputs QtHelp document'
arch=(
  any
)
_http="https://github.com"
_ns="sphinx-doc"
url="${_http}/${_ns}/${_pkg}"
license=(
  BSD-2-Clause
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  git
  "${_py}-build"
  "${_py}-flit-core"
  "${_py}-installer"
)
checkdepends=(
  "${_py}-defusedxml"
  "${_py}-pytest"
  "${_py}-sphinx"
)
source=(
  "${_pkg}-${pkgver}::git+${url}.git#tag=$pkgver"
)
b2sums=(
  'e7b102bad614c0793a6aeed26fce0260b06553045fec1f5bd69d1df850cfb8692e635e70ebedb0531447fe984234234b763a3cb5b1b89034071ac9da2fffab3d'
)

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  PYTHONPATH="sphinxcontrib/devhelp:${PYTHONPATH}" \
  pytest
}

package() {
  local \
    _site_packages
  _site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # Symlink license file
  install \
    -dm755 \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_Pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
