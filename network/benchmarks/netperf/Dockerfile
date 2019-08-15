# Copyright 2016 The Kubernetes Authors All rights reserved.
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

# network performance tests in containers
#
# Run as the Orchestrator: Arguments: --mode=orchestrator
#
# Run as a Worker (first get orchestrator virtual IP address):
# kubectl get svc --format "{{ .NetworkSettings.IPAddress }}" netperf-orch
#
# Args: --mode=worker --host=<service cluster ip> --port=5202
#
FROM debian

# install binary and remove cache
RUN apt-get update \
    && apt-get install -y build-essential texinfo curl wget net-tools autoconf automake git \
    && rm -rf /var/lib/apt/lists/*
RUN mkdir -p /tmp

# Download and build perf tools from source
RUN git clone --no-checkout --depth 1 https://github.com/HewlettPackard/netperf/
RUN cd netperf && \
    git checkout bcb868bde7f0203bbab69609f65d4088ba7398db && \
    ./autogen.sh && \
    ./configure --prefix=/usr --bindir /usr/bin && \
    make && \
    make install

RUN git clone --depth 1 https://github.com/esnet/iperf/ -b 3.7
RUN cd iperf && \
    ./configure --prefix=/usr --bindir /usr/bin && \
    make && \
    make install

COPY nptests /usr/bin/

ENTRYPOINT ["nptests"]
