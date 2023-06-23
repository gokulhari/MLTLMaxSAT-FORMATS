FROM ubuntu:22.04
RUN echo 'APT::Install-Suggests "0";' >> /etc/apt/apt.conf.d/00-docker
RUN echo 'APT::Install-Recommends "0";' >> /etc/apt/apt.conf.d/00-docker
RUN DEBIAN_FRONTEND=noninteractive \
  apt-get update \
  && apt-get install -y python3 \
  && apt-get install -y build-essential \
  && apt-get install -y wget \
  && apt-get install -y git \
  && apt-get install -y openjdk-11-jre \
  && apt-get install -y python3-pip \
  && rm -rf /var/lib/apt/lists/* 

RUN mkdir /home/gokul && \
cd /home/gokul && \
git config --global http.sslverify false && \
git clone https://github.com/gokulhari/MLTLMaxSAT-FORMATS.git && \
wget --no-check-certificate https://github.com/Kitware/CMake/releases/download/v3.22.1/cmake-3.22.1-linux-x86_64.sh && \
yes | bash cmake-3.22.1-linux-x86_64.sh

RUN cd /home/gokul && \ 
cd MLTLMaxSAT-FORMATS && \
mkdir build && \
cd build && \
/home/gokul/cmake-3.22.1-linux-x86_64/bin/cmake .. && \
make -j12

RUN cd /home/gokul/ && \
wget https://temporallogic.org/research/CAV19/artifact.tar.xz && \
tar -xf artifact.tar.xz && \
pip install matplotlib