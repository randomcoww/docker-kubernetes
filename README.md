### Image build

```
mkdir -p build
export TMPDIR=$(pwd)/build

GO_VERSION=1.17
VERSION=v1.22.4

podman build \
  --build-arg VERSION=$VERSION \
  --build-arg GO_VERSION=$GO_VERSION \
  -f Dockerfile.base \
  -t ghcr.io/randomcoww/kubernetes:base-$VERSION

podman build \
  --build-arg VERSION=$VERSION \
  -f Dockerfile.kube-master \
  -t ghcr.io/randomcoww/kubernetes:kube-master-$VERSION

podman build \
  --build-arg VERSION=$VERSION \
  -f Dockerfile.kube-proxy \
  -t ghcr.io/randomcoww/kubernetes:kube-proxy-$VERSION

podman build \
  --build-arg VERSION=$VERSION \
  -f Dockerfile.kubelet \
  -t ghcr.io/randomcoww/kubernetes:kubelet-$VERSION
```

```
podman push ghcr.io/randomcoww/kubernetes:kube-master-$VERSION
podman push ghcr.io/randomcoww/kubernetes:kube-proxy-$VERSION
podman push ghcr.io/randomcoww/kubernetes:kubelet-$VERSION
```