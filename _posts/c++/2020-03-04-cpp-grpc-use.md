---
layout: post
title: C++ 에서 Grpc 사용하기 (우분투 16.04)
tags: [c++, programming, effective]
category : C++
---

> https://github.com/grpc/grpc/tree/master/src/cpp

> cmake가 3.15 이상으로 유지할것! 그렇지 않으면 gRPCTarget.cmake 파일이 /usr/local/lib/cmake 경로에 제대로 생기지 않는다.

# Grpc 설치

```
### Linux
 $ [sudo] apt-get install build-essential autoconf libtool pkg-config
If you plan to build using CMake

 $ [sudo] apt-get install cmake
If you are a contributor and plan to build and run tests, install the following as well:

 $ # libgflags-dev is only required if building with make (deprecated)
 $ [sudo] apt-get install libgflags-dev
 $ # clang and LLVM C++ lib is only required for sanitizer builds
 $ [sudo] apt-get install clang-5.0 libc++-dev
 $ [sudo] apt-get install -y libunwind-dev
$ [sudo] apt-get install golang

```

```
$ apt-get install git curl
$ git clone -b $(curl -L http://grpc.io/release) https://github.com/grpc/grpc
$ cd grpc/
$ git submodule update --init
```

```
$ mkdir -p cmake/build
$ cd cmake/build
$ cmake  -DgRPC_INSTALL=ON -DgRPC_ZLIB_PROVIDER=package -DgRPC_CARES_PROVIDER=package -DgRPC_PROTOBUF_PROVIDER=package -DgRPC_SSL_PROVIDER=package ../..
$ make
sudo make install
```

# Example 실행
> https://github.com/grpc/grpc/tree/master/examples/cpp/helloworld

위 주소에서 clone하여 서버와 클라이언트를 실행할 수 있다.

protoc가 제대로 깔려있다면
```
protoc -I ../../protos/ --grpc_out=. --plugin=protoc-gen-grpc=grpc_cpp_plugin ../../protos/helloworld.proto
```
을 사용하여 proto 파일을 c++ 파일에서 사용할 수 있도록 컴파일한다.

