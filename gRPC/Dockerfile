FROM golang:latest

MAINTAINER gnfy

RUN mkdir /opt/protoc && \
    sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list  && \
    sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    apt-get update && \
    apt-get install --assume-yes apt-utils unzip curl

# 下载protobuf
RUN curl -sfL https://github.com/protocolbuffers/protobuf/releases/download/v3.19.1/protoc-3.19.1-linux-x86_64.zip -o protoc.zip

#解压
RUN unzip -o protoc.zip -d /opt/protoc

RUN go env -w GOPROXY=https://goproxy.cn && \
    echo export PATH="$PATH:$(go env GOPATH)/bin:/opt/protoc/bin" >> ~/.bashrc

# Install grpc
RUN go get -u google.golang.org/grpc \
    && go get -u github.com/golang/protobuf/protoc-gen-go \
    && go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway
