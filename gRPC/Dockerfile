FROM golang:latest

MAINTAINER gnfy

RUN mkdir /opt/protoc

# 需要预先下载protoc文件,并且解压到当前的文件夹下,地址：https://github.com/protocolbuffers/protobuf/releases/download/v3.19.1/protoc-3.19.1-linux-x86_64.zip

COPY protoc-3.19.1-linux-x86_64 /opt/protoc

RUN go env -w GOPROXY=https://goproxy.cn \
    && echo export PATH="$PATH:$(go env GOPATH)/bin:/opt/protoc/bin" >> ~/.bashrc

# Install grpc
RUN go get -u google.golang.org/grpc \
    && go get -u github.com/golang/protobuf/protoc-gen-go \
    && go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway
