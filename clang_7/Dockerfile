FROM ubuntu:cosmic

LABEL maintainer="Luis Martinez de Bartolome <luism@jfrog.com>"

ENV LLVM_VERSION=7.0 \
    CC=clang \
    CXX=clang++ \
    CMAKE_C_COMPILER=clang \
    CMAKE_CXX_COMPILER=clang++ \
    PYENV_ROOT=/opt/pyenv \
    PATH=/opt/pyenv/shims:${PATH}

RUN dpkg --add-architecture i386 \
    && apt-get -qq update \
    && apt-get install -y --no-install-recommends \
       sudo=1.8.23-2ubuntu1 \
       wget=1.19.5-1ubuntu1 \
       git \
       vim=2:8.0.1766-1ubuntu1 \
       g++-multilib=4:8.2.0-1ubuntu1 \
       clang-7\
       make=4.2.1-1.2 \
       valgrind=1:3.13.0-2ubuntu6 \
       libc6-dev-i386=2.28-0ubuntu1 \
       libgmp-dev=2:6.1.2+dfsg-3 \
       libmpfr-dev=4.0.1-1 \
       libmpc-dev=1.1.0-1 \
       nasm=2.13.03-2 \
       dh-autoreconf=19 \
       libffi-dev=3.2.1-8 \
       libssl-dev=1* \
       ninja-build=1.8.2-1 \
       libc++-dev=6.0.1-1 \
       libc++-dev:i386=6.0.1-1 \
       libc++abi-dev=6.0.1-1 \
       libc++abi-dev:i386=6.0.1-1 \
       pkg-config=0.29.1-0ubuntu2 \
       subversion=1.10.0-2ubuntu2 \
       zlib1g-dev=1:1.* \
       libbz2-dev=1.* \
       libsqlite3-dev=3.* \
       libreadline-dev=7.* \
       xz-utils=5.* \
       curl=7.* \
       libncurses5-dev=6.* \
       libncursesw5-dev=6.* \
       liblzma-dev=5.* \
       ca-certificates=20180409 \
    && update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-7 100 \
    && update-alternatives --install /usr/bin/clang clang /usr/bin/clang-7 100 \
    && update-alternatives --install /usr/bin/cc cc /usr/bin/clang-7 100 \
    && update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++-7 100 \
    && update-alternatives --install /usr/bin/cpp cpp /usr/bin/clang++-7 100 \
    && ln -s /usr/include/locale.h /usr/include/xlocale.h \
    && rm -rf /var/lib/apt/lists/* \
    && wget -q --no-check-certificate https://cmake.org/files/v3.12/cmake-3.12.1-Linux-x86_64.tar.gz \
    && tar -xzf cmake-3.12.1-Linux-x86_64.tar.gz \
    && cp -fR cmake-3.12.1-Linux-x86_64/* /usr \
    && rm -rf cmake-3.12.1-Linux-x86_64 \
    && rm cmake-3.12.1-Linux-x86_64.tar.gz \
    && groupadd 1001 -g 1001 \
    && groupadd 1000 -g 1000 \
    && groupadd 2000 -g 2000 \
    && groupadd 999 -g 999 \
    && useradd -ms /bin/bash conan -g 1001 -G 1000,2000,999 \
    && printf "conan:conan" | chpasswd \
    && adduser conan sudo \
    && printf "conan ALL= NOPASSWD: ALL\\n" >> /etc/sudoers \
    && wget --no-check-certificate --quiet -O /tmp/pyenv-installer https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer \
    && chmod +x /tmp/pyenv-installer \
    && /tmp/pyenv-installer \
    && rm /tmp/pyenv-installer \
    && update-alternatives --install /usr/bin/pyenv pyenv /opt/pyenv/bin/pyenv 100 \
    && pyenv install 3.7.1 \
    && pyenv global 3.7.1 \
    && pip install -q --upgrade --no-cache-dir pip \
    && pip install -q --no-cache-dir conan \
    && chown -R conan:1001 /opt/pyenv \
    && update-alternatives --install /usr/bin/python python /opt/pyenv/shims/python 100 \
    && update-alternatives --install /usr/bin/python3 python3 /opt/pyenv/shims/python3 100 \
    && update-alternatives --install /usr/bin/pip pip /opt/pyenv/shims/pip 100 \
    && update-alternatives --install /usr/bin/pip3 pip3 /opt/pyenv/shims/pip3 100

USER conan
WORKDIR /home/conan

RUN mkdir -p /home/conan/.conan \
    && printf 'eval "$(pyenv init -)"\n' >> ~/.bashrc \
    && printf 'eval "$(pyenv virtualenv-init -)"\n' >> ~/.bashrc
