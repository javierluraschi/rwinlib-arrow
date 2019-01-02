# Maintainer: Kouhei Sutou <kou@clear-code.com>

_realname=arrow
pkgbase=mingw-w64-${_realname}-backport
pkgname="${MINGW_PACKAGE_PREFIX}-${_realname}"
pkgver=0.12.0
pkgrel=1
pkgdesc="Apache Arrow is a cross-language development platform for in-memory data (mingw-w64)"
arch=("any")
url="https://arrow.apache.org/"
license=("Apache-2.0")
depends=("${MINGW_PACKAGE_PREFIX}-boost"
         "${MINGW_PACKAGE_PREFIX}-zlib")
makedepends=("${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-gcc")
options=("staticlibs" "strip" "!buildflags")
source=("https://github.com/javierluraschi/arrow/archive/apache-arrow-${pkgver}.tar.gz"
        "cpuidex.patch")
sha256sums=("SKIP"
            "SKIP")

cmake_build_type=release
meson_build_type=debugoptimized

source_dir=arrow-apache-${_realname}-${pkgver}
cpp_build_dir=build-${CARCH}-cpp
c_glib_build_dir=build-${CARCH}-c-glib

prepare() {
  pushd ${source_dir}
  patch -p1 -i ${srcdir}/cpuidex.patch
  popd
}

build() {
  [[ -d ${cpp_build_dir} ]] && rm -rf ${cpp_build_dir}
  mkdir -p ${cpp_build_dir}
  pushd ${cpp_build_dir}

  export CC="/C/Rtools${MINGW_PREFIX/mingw/mingw_}/bin/gcc"
  export CXX="/C/Rtools${MINGW_PREFIX/mingw/mingw_}/bin/g++"
  export PATH="/C/Rtools${MINGW_PREFIX/mingw/mingw_}/bin:$PATH"
  export CPPFLAGS="-I${MINGW_PREFIX}/include"
  export LIBS="-L${MINGW_PREFIX}/libs"

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
    ${MINGW_PREFIX}/bin/cmake.exe \
      ../${source_dir}/cpp \
      -G "MSYS Makefiles" \
      -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
      -DCMAKE_BUILD_TYPE=${cmake_build_type} \
    -DARROW_BUILD_STATIC=ON \
      -DARROW_BUILD_TESTS=OFF \
      -DARROW_PYTHON=OFF \
      -DARROW_BOOST_USE_SHARED=OFF \
      -DARROW_WITH_SNAPPY=OFF \
    -DARROW_WITH_ZSTD=OFF \
    -DARROW_WITH_LZ4=OFF \
    -DARROW_JEMALLOC=OFF \
    -DARROW_BUILD_SHARED=OFF\
    -DARROW_BOOST_VENDORED=OFF \
    -DARROW_WITH_ZLIB=OFF \
    -DARROW_WITH_BROTLI=OFF \
    -DARROW_USE_GLOG=OFF \
    -DPTHREAD_LIBRARY=OFF \
    -DARROW_BUILD_UTILITIES=ON \
    -DARROW_HDFS=OFF \
    -DARROW_TEST_LINKAGE=static \
    -DARROW_IPC=ON

    sed -i 's/-fPIC/ /g' flatbuffers_ep-prefix/src/flatbuffers_ep-stamp/flatbuffers_ep-configure-RELEASE.cmake

  make
  popd
}

check() {
  # TODO
  # make -C ${cpp_build_dir} test

  # PATH=$(pwd)/${c_glib_build_dir}/arrow-glib:$(pwd)/${cpp_build_dir}/${cmake_build_type}:$PATH \
  #   ninja -C ${c_glib_build_dir} test

  :
}

package() {
  make -C ${cpp_build_dir} DESTDIR="${pkgdir}" install

  local PREFIX_DEPS=$(cygpath -am ${MINGW_PREFIX})
  pushd "${pkgdir}${MINGW_PREFIX}/lib/pkgconfig"
  for pc in *.pc; do
    sed -s "s|${PREFIX_DEPS}|${MINGW_PREFIX}|g" -i $pc
  done
  popd
}
