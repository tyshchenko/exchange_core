FROM ubuntu:16.04
MAINTAINER Sergey Dolin <sergey@s4y.solutions>

# Install all necessary for building dependecnies, read more about
# depencensies in readme section in the root of the repo.
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
      jq \
      netcat \
      inetutils-ping \
      vim \
      git \
      wget \
      make \
      cmake \
      gcc \
      g++ \
      libldap-2.4 \
      libgss3 \
      libcurl3 \
      librtmp1 \
      libmysqlclient20 \
      python \
      libev-dev \
      libmpdec-dev \
      libjansson-dev \
      libssl-dev \
      libgnutls-dev \
      libmysqlclient-dev \
      libhttp-parser-dev \
      libcurl4-openssl-dev \
      liblz4-dev \
	  libldap2-dev \
	  libkrb5-dev \
	  libalberta-dev  \
	  libgss-dev \
	  libidn11-dev \
	  librtmp-dev &&\
    rm -rf /var/lib/apt/lists/* 



RUN git clone https://github.com/edenhill/librdkafka.git \
    && cd librdkafka \
    && ./configure \
    && make \
    && make install \
    && ldconfig \
    && cd / \
    && rm -rf librdkafka

# RUN LD_LIBRARY_PATH=/usr/local/lib \
#    && export LD_LIBRARY_PATH

RUN mkdir viabtc_exchange_server

# Copy all necessary files and directories for build process, basically we
# recreate the root repository directory, but with tham we could ensure that
# nothing
COPY depends    viabtc_exchange_server/depends
COPY network    viabtc_exchange_server/network
COPY utils      viabtc_exchange_server/utils

COPY alertcenter    viabtc_exchange_server/alertcenter
COPY matchengine    viabtc_exchange_server/matchengine
COPY marketprice    viabtc_exchange_server/marketprice
COPY readhistory    viabtc_exchange_server/readhistory
COPY accesshttp     viabtc_exchange_server/accesshttp
COPY accessws       viabtc_exchange_server/accessws

COPY makefile.inc                                   viabtc_exchange_server/makefile.inc

# Do the actial build of the microservices.
RUN cd viabtc_exchange_server/depends/hiredis \
    && make \
    && mv libhiredis.* /usr/lib \
    && cd .. \
    && mv hiredis /usr/include

RUN cd viabtc_exchange_server/network \
    && make

RUN cd viabtc_exchange_server/utils \
    && make

RUN cd viabtc_exchange_server/alertcenter \
    && make
RUN cp /viabtc_exchange_server/alertcenter/alertcenter.exe \
       /viabtc_exchange_server/alertcenter.exe

RUN cd viabtc_exchange_server/matchengine \
    && make
RUN cp /viabtc_exchange_server/matchengine/matchengine.exe \
       /viabtc_exchange_server/matchengine.exe

RUN cd viabtc_exchange_server/marketprice \
    && make
RUN cp /viabtc_exchange_server/marketprice/marketprice.exe \
       /viabtc_exchange_server/marketprice.exe

RUN cd viabtc_exchange_server/readhistory \
    && make
RUN cp /viabtc_exchange_server/readhistory/readhistory.exe \
       /viabtc_exchange_server/readhistory.exe

RUN cd viabtc_exchange_server/accesshttp \
    && make
RUN cp /viabtc_exchange_server/accesshttp/accesshttp.exe \
       /viabtc_exchange_server/accesshttp.exe

RUN cd viabtc_exchange_server/accessws \
    && make
RUN cp /viabtc_exchange_server/accessws/accessws.exe \
       /viabtc_exchange_server/accessws.exe


COPY docker/exchange/configs/alertcenter.json /viabtc_exchange_server
COPY docker/exchange/configs/matchengine.json /viabtc_exchange_server
COPY docker/exchange/configs/marketprice.json /viabtc_exchange_server
COPY docker/exchange/configs/readhistory.json /viabtc_exchange_server
COPY docker/exchange/configs/accesshttp.json /viabtc_exchange_server
COPY docker/exchange/configs/accessws.json /viabtc_exchange_server

COPY docker/exchange/entrypoint.sh /docker-entrypoint.sh
ENTRYPOINT [ "/docker-entrypoint.sh" ]
