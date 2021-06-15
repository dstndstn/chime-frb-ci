FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt -y update && \
    apt install -y --no-install-recommends apt-utils && \
    apt install -y --no-install-recommends \
    make \
    g++ \
    git \
    file \
    pkg-config \
    wget \
    python \
    python-dev \
    ca-certificates \
    libhdf5-dev \
    python-pip \
    cython \
    libfftw3-dev \
    libmsgpack-dev \
    libzmqpp-dev \
    libjsoncpp-dev \
    libcurl4-gnutls-dev \
    liblz4-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN pip install numpy

RUN mkdir /src
WORKDIR /src

ENV INCDIR /src/include
ENV LIBDIR /src/lib
ENV BINDIR /src/bin
ENV PYDIR /src/lib/python2.7/site-packages
ENV HDF5_INC -I/usr/include/hdf5/serial
ENV HDF5_LIB -L/usr/lib/x86_64-linux-gnu/hdf5/serial
ENV NUMPY_INCDIR /usr/local/lib/python2.7/dist-packages/numpy/core/include

RUN git clone https://github.com/kmsmith137/simd_helpers.git
COPY Makefile.local-simd-helpers /src/simd_helpers/Makefile.local
RUN cd simd_helpers && \
    make -j8 && make install

RUN git clone https://github.com/kmsmith137/sp_hdf5.git
COPY Makefile.local-sp-hdf5 /src/sp_hdf5/Makefile.local
RUN cd sp_hdf5 && \
    make -j8 && make install

RUN git clone https://github.com/kmsmith137/simpulse.git
COPY Makefile.local-simpulse /src/simpulse/Makefile.local
RUN cd simpulse && make -j8 && make install

RUN git clone https://github.com/kmsmith137/pyclops.git
COPY Makefile.local-pyclops /src/pyclops/Makefile.local
COPY pyclops.patch /src/pyclops
RUN cd pyclops && patch -p1 < pyclops.patch
RUN cd pyclops && make -j8 && make install

RUN git clone https://github.com/kmsmith137/rf_kernels.git
#RUN cd rf_kernels && git checkout dstn-fetch-spline
COPY Makefile.local-rf-kernels /src/rf_kernels/Makefile.local
RUN cd rf_kernels && make -j8 && make install


