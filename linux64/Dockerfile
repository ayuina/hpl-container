#https://sites.google.com/site/rangsiman1993/comp-env/test-and-benchmarks/build-hpl-with-blas-and-mpi
FROM ubuntu:18.04 
WORKDIR /src
# Install
RUN apt-get update \
    && apt-get install -y build-essential wget ssh
# OpenBLAS
WORKDIR /src
RUN wget https://github.com/xianyi/OpenBLAS/archive/v0.3.10.tar.gz \
    && tar xzvf v0.3.10.tar.gz  \
    && cd OpenBLAS-0.3.10 \
    && make \
    && make PREFIX=/src/blas install
# OpenMPI
WORKDIR /src
RUN wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.3.tar.gz \
    && tar xzvf openmpi-3.1.3.tar.gz \
    && cd openmpi-3.1.3 \
    && ./configure --prefix=/src/mpi/ \
    && make all install 
# HPL
WORKDIR /src
ENV PATH $PATH:/src/mpi/bin 
ENV LD_LIBRARY_PATH /src/mpi/lib/libmpi.so
COPY . . 
RUN wget http://www.netlib.org/benchmark/hpl/hpl-2.3.tar.gz \
    && tar zxvf hpl-2.3.tar.gz \
    && cd ./hpl-2.3 \
    && cp ../Make.linux64 ./ \
    && make arch=linux64 \
    && cp ../HPL.dat ./bin/linux64/

