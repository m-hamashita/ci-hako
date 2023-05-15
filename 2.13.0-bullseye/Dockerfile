FROM cimg/ruby:2.5.5 AS builder

RUN apt-get update && apt-get install -y --no-install-recommends git \
      && git clone https://github.com/google/jsonnet.git \
      && cd jsonnet \
      && make libjsonnet.so \
      && mkdir -p /opt/jsonnet \
      && cp libjsonnet.so /opt/jsonnet/libjsonnet.so \
      && cp libjsonnet.so.0 /opt/jsonnet/libjsonnet.so.0 \
      && cp include/libjsonnet.h /opt/jsonnet/libjsonnet.h \
      && cd .. \
      && rm -rf jsonnet \
      && apt-get autoremove -y g++ && apt-get clean && rm -rf /var/lib/apt/lists/*

# ruby:2.5-alpine3.8 -> ruby 2.5.5
FROM cimg/ruby:2.5.5
COPY --from=builder /opt/jsonnet/libjsonnet.so /usr/local/lib/libjsonnet.so
COPY --from=builder /opt/jsonnet/libjsonnet.so.0 /usr/local/lib/libjsonnet.so.0
COPY --from=builder /opt/jsonnet/libjsonnet.h /usr/local/include/libjsonnet.h

ENV HAKO_VERSION=2.13.0

RUN apt-get update && apt-get install -y openssh-client git zip make docker g++ \
      && JSONNET_USE_SYSTEM_LIBRARIES=1 gem install hako -v ${HAKO_VERSION} -N \
      && gem install specific_install -N && gem specific_install https://github.com/moaikids/hako-parameterstore.git -r 8ed6452bd15e24c01b0143de6bccb5bfa86942a4 \
      && apt-get autoremove -y g++ && apt-get clean && rm -rf /var/lib/apt/lists/*
