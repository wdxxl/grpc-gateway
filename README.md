####
```
go mod init grpc-gateway
export GOPROXY=https://goproxy.cn,direct
export GOSUMDB=off
go mod tidy
go mod vendor
```

#### 
```
export GOPROXY=https://goproxy.cn,direct
export GOSUMDB=off

chmod 777 install-grpc-gateway.sh 
./install-grpc-gateway.sh      
```

#### Proto -> GRPC
```
protoc -I. \
-I$GOPATH/src \
-I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
--go_out=plugins=grpc:. \
proto/helloworld.proto
```

#### Proto -> gRPC-Gateway
```
protoc -I. \
-I$GOPATH/src \
-I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
--grpc-gateway_out=logtostderr=true:. \
proto/helloworld.proto
```

#### Proto -> Protoset
```
protoc --proto_path=. --include_imports \
-I=$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis:. \
--descriptor_set_out=proto/helloworld.protoset \
proto/helloworld.proto
```

#### Proto -> Swagger
```
protoc -I. \
-I$GOPATH/src \
-I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
--swagger_out=logtostderr=true:. \
proto/helloworld.proto
```


##### 1. ---> Gprc - Gateway curl
```
curl -X POST -d '{"name": "wdxxl"}' localhost:9090/helloworld
```

##### 2. ---> Grpcurl proto
```
grpcurl -v -d '{"name": "wdxxl"}' -proto proto/helloworld.proto -plaintext localhost:50051 helloworld.Greeter.SayHello
报错:
Failed to process proto source files.: could not parse given files: proto/helloworld.proto:5:8: open google/api/annotations.proto: no such file or directory
```

##### 3. ---> Grpcurl protoset
```
grpcurl -v -d '{"name": "wdxxl-protoset"}' -protoset proto/helloworld.protoset -plaintext localhost:50051 helloworld.Greeter.SayHello
```
