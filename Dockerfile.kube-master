FROM golang:1.13-alpine as BUILD

WORKDIR /go/src/github.com/kubernetes
ENV KUBERNETES_VERSION v1.17.0

RUN set -x \
  \
  && apk add --no-cache \
    git \
    make \
    bash \
    rsync \
    g++ \
    linux-headers \
  \
  && git clone -b $KUBERNETES_VERSION \
    https://github.com/kubernetes/kubernetes.git \
  && cd kubernetes \
  && make -j "$(getconf _NPROCESSORS_ONLN)" \
    kube-apiserver \
    kube-controller-manager \
    kube-scheduler \
    kube-proxy \
    kubelet

FROM scratch

COPY --from=BUILD /go/src/github.com/kubernetes/kubernetes/_output/bin/kube-apiserver /usr/local/bin/
COPY --from=BUILD /go/src/github.com/kubernetes/kubernetes/_output/bin/kube-controller-manager /usr/local/bin/
COPY --from=BUILD /go/src/github.com/kubernetes/kubernetes/_output/bin/kube-scheduler /usr/local/bin/