FROM ruby:3.1.4-bullseye

ENV HAKO_VERSION=2.13.0

RUN apt-get update && apt-get install -y openssh-client git zip make docker g++ \
      && git clone https://github.com/google/jsonnet.git \
      && cd jsonnet \
      && make libjsonnet.so \
      && cp libjsonnet.so /usr/local/lib/libjsonnet.so \
      && cp libjsonnet.so.0 /usr/local/lib/libjsonnet.so.0 \
      && cp include/libjsonnet.h /usr/local/include/libjsonnet.h \
      && cd .. \
      && rm -rf jsonnet

RUN JSONNET_USE_SYSTEM_LIBRARIES=1 gem install hako -v ${HAKO_VERSION} -N \
      && gem install specific_install -N && gem specific_install https://github.com/moaikids/hako-parameterstore.git -r 8ed6452bd15e24c01b0143de6bccb5bfa86942a4 \
      && apt-get autoremove -y g++ && apt-get clean && rm -rf /var/lib/apt/lists/*
