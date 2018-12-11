- Create EC2 Windows Machine and connect.
- Download and install [MSys2](https://www.msys2.org/).
- Launch `c:\msys64\mingw64` and run:

```
pacman -S base-devel
pacman -S msys2-devel
pacman -S mingw-w64-i686-toolchain
pacman -S mingw-w64-x86_64-toolchain
pacman -S mingw-w64-x86_64-cmake
```

- Download boost from [boost_1_68_0.zip](https://www.boost.org/users/download/).
- Extract into `c:\Users\Administrator\RStudio\boost_1_68_0`.
- Build boost from `mingw64` shell as follows:

```
cd /c/Users/Administrator/RStudio/boost_1_68_0/
./bootstrap.sh --with-libraries=regex,system,filesystem
./b2 -j2 variant=release link=static
```

- [Download GitHub SCM for Windows](https://git-scm.com/downloads).
- From Windows command prompt, checkout rarrow and Apache Arrow:

```
cd c:\Users\Administrator\RStudio\
git clone http://github.com/apache/arrow
git checkout f9e5818ef8bd4f1eea13c1185a59077bb1323374
```

- From the `mingw64` shell, build arrow:

```
cd /c/Users/Administrator/RStudio/arrow/cpp
mkdir release
cd release
export BOOST_ROOT=/c/Users/Administrator/RStudio/boost_1_68_0/
cmake .. -DARROW_BUILD_TESTS=FALSE -DARROW_WITH_SNAPPY=FALSE -DARROW_WITH_ZSTD=FALSE -DARROW_WITH_LZ4=FALSE -DARROW_WITH_LZ4=FALSE -DARROW_JEMALLOC=FALSE -DARROW_BUILD_STATIC=TRUE -DARROW_BOOST_VENDORED=FALSE -DARROW_BOOST_USE_SHARED=FALSE -DARROW_WITH_ZLIB=FALSE -DARROW_WITH_BROTLI=FALSE -DARROW_USE_GLOG=FALSE -DPTHREAD_LIBRARY=FALSE -DARROW_BUILD_UTILITIES=FALSE -DCMAKE_BUILD_TYPE=release -G"MSYS Makefiles" -DCMAKE_CXX_COMPILER_ARCHITECTURE_ID=x64 -DARROW_HDFS=FALSE
cmake .. -DARROW_BUILD_TESTS=FALSE -DARROW_WITH_SNAPPY=FALSE -DARROW_WITH_ZSTD=FALSE -DARROW_WITH_LZ4=FALSE -DARROW_WITH_LZ4=FALSE -DARROW_JEMALLOC=FALSE -DARROW_BUILD_STATIC=TRUE -DARROW_BOOST_VENDORED=FALSE -DARROW_BOOST_USE_SHARED=FALSE -DARROW_WITH_ZLIB=FALSE -DARROW_WITH_BROTLI=FALSE -DARROW_USE_GLOG=FALSE -DPTHREAD_LIBRARY=FALSE -DARROW_BUILD_UTILITIES=FALSE -DCMAKE_BUILD_TYPE=release -G"MSYS Makefiles" -DCMAKE_CXX_COMPILER_ARCHITECTURE_ID=x64 -DARROW_HDFS=FALSE
make install
```

- Update `rwinlib/arrow` binaries:

```
cd <rwinlib/arrow github path>
cp -r /c/Program\ Files\ \(x86\)/arrow/include .
cp /c/Program\ Files\ \(x86\)/arrow/lib/libarrow.a lib/x64/
```

- Update `rwinlib/arrow` compiler:
- Download https://nuwen.net/files/mingw/history/mingw-15.4.exe
- Extract to `rwinlib/arrow/`, optionally, delete unnecesary libraries.
