# UDF Server Demo base on Boost

## 编译方式

```shell
cd src
mkdir build && cd build
make -j
```

## 文件说明

`process_client.cpp` : client端

`process_server.cpp` : server端

`shm_manager.hpp` : 共享内存管理器

编译后会有可执行文件./client和./server

只用运行./client就可以了


## 部分环境安装

### boost install

```shell
./bootstrap.sh
./b2 install cxxflags="-std=c++14"
```

### Arrow

docker pull apache/arrow-dev:amd64-ubuntu-20.04-r-4.4

```shell
git clone -b release-16.1.0-rc0 https://github.com/apache/arrow.git
git submodule update --init
cd cpp
mkdir build && cd build
cmake -DARROW_CSV=ON -DARROW_JSON=ON -DARROW_FILESYSTEM=ON -DARROW_COMPUTE=ON -DARROW_DATASET=ON -DARROW_PARQUET=ON -DPARQUET_REQUIRE_ENCRYPTION=ON  ..
make -j32 && make install

cd python
PYARROW_WITH_COMPUTE=1 PYARROW_PARALLEL=32 PYARROW_WITH_CSV=1 PYARROW_WITH_JSON=1 PYARROW_WITH_FILESYSTEM=1 PYARROW_WITH_DATASET=1 PYARROW_WITH_PARQUET=1 python setup.py build_ext --bundle-arrow-cpp bdist_wheel
```


### 按需环境安装

```shell
# abseil-cpp
git clone https://github.com/abseil/abseil-cpp.git
cd abseil-cpp && mkdir build && cd build
cmake -DCMAKE_CXX_FLAGS="-fPIC" .. && make -j32 && make install

# re2
git clone https://github.com/google/re2.git
cd re2 && mkdir build && cd build
cmake -DCMAKE_CXX_FLAGS="-fPIC" .. && make -j32 && make install

# xsimd
git clone https://github.com/xtensor-stack/xsimd.git
cd xsimd && mkdir build && cd build
cmake .. && make -j32 && make install

git clone https://github.com/JuliaStrings/utf8proc.git
cd utf8proc && mkdir build && cd build
cmake .. && make -j32 && make install
```