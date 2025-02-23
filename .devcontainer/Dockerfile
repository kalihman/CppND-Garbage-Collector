# Copyright 2018 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:18.04 AS devtools

# Please keep the formatting in these commands, it is optimized to cut & paste
# into the README.md file.

## [START INSTALL.md]

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install the minimal development tools:

# ```bash
RUN apt update \
    && apt install -y --no-install-recommends apt-utils dialog 2>&1 \
    && apt install -y build-essential cmake git gcc g++ cmake procps lsb-release \
        libc-ares-dev libc-ares2 libcurl4-openssl-dev libssl-dev make \
        pkg-config tar wget zlib1g-dev cppcheck valgrind \
        libncurses5-dev libncursesw5-dev \
    # Create a non-root user to use if preferred
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # [Optional] Add sudo support for non-root user 
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# ```

# #### Protobuf

# While protobuf-3.0 is distributed with Ubuntu, the Google Cloud Plaform proto
# files require more recent versions (circa 3.4.x). To manually install a more
# recent version use:

# ```bash
# WORKDIR /var/tmp/build
# RUN wget -q https://github.com/google/protobuf/archive/v3.9.0.tar.gz
# RUN tar -xf v3.9.0.tar.gz
# WORKDIR /var/tmp/build/protobuf-3.9.0/cmake
# RUN cmake \
#         -DCMAKE_BUILD_TYPE=Release \
#         -DBUILD_SHARED_LIBS=yes \
#         -Dprotobuf_BUILD_TESTS=OFF \
#         -H. -Bcmake-out
# RUN cmake --build cmake-out --target install -- -j $(nproc)
# RUN ldconfig
# ```

# #### gRPC

# Likewise, Ubuntu has packages for grpc-1.3.x, but this version is too old for
# the Google Cloud Platform APIs:

# ```bash
# WORKDIR /var/tmp/build
# RUN wget -q https://github.com/grpc/grpc/archive/v1.22.0.tar.gz
# RUN tar -xf v1.22.0.tar.gz
# WORKDIR /var/tmp/build/grpc-1.22.0
# RUN make -j $(nproc)
# RUN make install
# RUN ldconfig
# ```

# FROM devtools AS install

# #### googleapis

# Finally we can install `googleapis`.

# ```bash
# WORKDIR /home/build/cpp-cmakefiles
# COPY . /home/build/cpp-cmakefiles
# RUN cmake -H. -Bcmake-out
# RUN cmake --build cmake-out -- -j $(nproc)
# WORKDIR /home/build/cpp-cmakefiles/cmake-out
# RUN cmake --build . --target install
# ```

## [END INSTALL.md]

# Verify that the installed files are actually usable
# RUN /home/build/cpp-cmakefiles/ci/test-install/compile-test-projects.sh