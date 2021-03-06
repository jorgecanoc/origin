#
# This is the image that controls the standard build environment for releasing OpenShift Origin.
# It is responsible for performing a cross platform build of OpenShift.
#
# The standard name for this image is openshift/origin-release
#
FROM openshift/origin-base

ENV VERSION=1.4.2 \
    GOARM=5 \
    GOPATH=/go \
    GOROOT=/usr/lib/golang \
    OS_VERSION_FILE=/go/src/github.com/openshift/origin/os-version-defs \
    TMPDIR=/openshifttmp
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

RUN mkdir $TMPDIR && \
    INSTALL_PKGS="make gcc zip mercurial golang golang-vet golang-cover golang-pkg-darwin-amd64 golang-pkg-windows-amd64 golang-pkg-linux-386" && \
    yum install -y $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all && \
    go get github.com/tools/godep && \
    touch /os-build-image

WORKDIR /go/src/github.com/openshift/origin
LABEL io.k8s.display-name="OpenShift Origin Release Environment (golang-$VERSION)" \
      io.k8s.description="This is the standard release image for OpenShift Origin and contains the necessary build tools to build the platform."

# Expect a tar with the source of OpenShift Origin (and /os-version-defs in the root)
# This image is for OpenShift versions v1.2.0 and earlier.
CMD bsdtar mxzf - && hack/build-cross.sh
