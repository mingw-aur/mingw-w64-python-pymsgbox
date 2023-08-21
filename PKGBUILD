# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Contributor: Alexey Pavlov <alexpux@gmail.com>
# Contributor: Ray Donnelly <mingw.android@gmail.com>
# Contributor: Duong Pham <dthpham@gmail.com>

_realname=pymsgbox
pkgbase=mingw-w64-python-${_realname}
pkgname=(
  "${MINGW_PACKAGE_PREFIX}-python-${_realname}")
provides=(
  "${MINGW_PACKAGE_PREFIX}-python3-${_realname}")
conflicts=(
  "${MINGW_PACKAGE_PREFIX}-python3-${_realname}")
replaces=(
  "${MINGW_PACKAGE_PREFIX}-python3-${_realname}")
pkgver=1.0.9
pkgrel=1
pkgdesc=(
  "Simple, cross-platform, pure Python module"
  "to display message boxes, and just message boxes")
pkgdesc="${_pkgdesc[*]}"
arch=('any')
mingw_arch=(
  'mingw32'
  'mingw64'
  'ucrt64'
  'clang64'
  'clang32'
  'clangarm64'
)
license=(
  'BSD'
)
url="https://${_realname}.readthedocs.io/en/latest/"
makedepends=(
  "${MINGW_PACKAGE_PREFIX}-python-setuptools")
depends=(
  "${MINGW_PACKAGE_PREFIX}-python"
  "${MINGW_PACKAGE_PREFIX}-openblas")
optdepends=()
# options=(
#   '!strip'
#   'debug')
_pypi_url="https://pypi.io/packages/source"
source=(
  "${_pypi_url}/${_realname::1}/${_realname}/${_realname}-${pkgver}.tar.gz")
sha512sums=(
  '929f2998aa5c26e238977815321911309eba64c3d9cbbe2354a02f9357e66c516cfb96b147b68fadbb543cf42d2060e7f2951a51f5f9f9af6cb8ea8da38a684e'
)

package() {
  local _mingw_prefix \
        _version_cmd=()
  _mingw_prefix=$(cygpath -am ${MINGW_PREFIX})
  _version_cmd=("import sys;"
	        "sys.stdout.write('.'.join(map(str,"
		                               "sys.version_info[:2])))"
  _pyver=$(${MINGW_PREFIX}/bin/python -c "${_version_cmd[*]}")
  _pyinc=${_pyver}
  cd "${_realname}"

  MSYS2_ARG_CONV_EXCL="--prefix=;--install-scripts=;--install-platlib=" \
  PYTHONPYCACHEPREFIX="${PWD}/.cache/cpython/" \
    "${MINGW_PREFIX}/python" -m installer \
                             --destdir="${pkgdir}" \
                             dist/*.whl

  install -Dm644 \
	  LICENSE.txt \
	  "${pkgdir}${MINGW_PREFIX}/share/licenses/python-${_realname}/LICENSE.txt"

  for _f in "${pkgdir}${MINGW_PREFIX}"/bin/*.py; do
    sed -e "s|${_mingw_prefix}/bin/|/usr/bin/env |g" \
	-i \
	${_f}
  done

  # Clean *.orig files
  find "${pkgdir}${MINGW_PREFIX}" \
       -type f \
       -name \
       "*.orig" \
       -exec rm -f {} \;
}

# vim:set sw=2 sts=-1 et:
