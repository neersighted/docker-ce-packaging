# syntax=docker/dockerfile:1

ARG GO_IMAGE
ARG DISTRO=fedora
ARG SUITE=37
ARG BUILD_IMAGE=${DISTRO}:${SUITE}

FROM ${GO_IMAGE} AS golang

FROM ${BUILD_IMAGE}
ENV GOPROXY=https://proxy.golang.org|direct
ENV GO111MODULE=off
ENV GOPATH /go
ENV PATH $PATH:/usr/local/go/bin:$GOPATH/bin
ENV AUTO_GOPATH 1
ARG DISTRO
ARG SUITE
ENV DISTRO=${DISTRO}
ENV SUITE=${SUITE}
RUN dnf install -y rpm-build rpmlint dnf-plugins-core
COPY --link SPECS /root/rpmbuild/SPECS
RUN dnf builddep -y /root/rpmbuild/SPECS/*.spec
COPY --link --from=golang /usr/local/go /usr/local/go
WORKDIR /root/rpmbuild
ENTRYPOINT ["/bin/rpmbuild"]
