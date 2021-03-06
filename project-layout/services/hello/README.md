## Compile proto:
Under the root of this project `hello`
```shell
protoc --go_out=. --go-grpc_out=. proto/hello.proto
```
or
```shell
protoc --go_out=. --go_opt=paths=source_relative --go-grpc_out=. --go-grpc_opt=paths=source_relative proto/proto.proto
```

## Build and Run:

### Option 1
+ open a terminal, starting server
```shell
cd server
go build && ./server
```
+ open another terminal, starting client
```shell
cd client
go build && ./client
```

### Option 2
+ open a terminal, starting server
```shell
cd server
go run server.go
```
+ open another terminal, starting client
```shell
cd client
go run client.go
```

## make a docker image and run

+ build a docker image
  + First Method: using a docker image to load the compiled executable binary file, please refer [simple dockerfile](Dockerfile-not-recommended)
    ```shell
    docker build -t chinlying/grpc-hello:V1.0 .
    ```
    + Shortage: It needs to be compiled locally, which is not suitable for cross platform
    + `Notes`: It's better not use `golang:1.1*` as the basic image, otherwise the compiled image will be huge (larger than 300MB) 
  + `Recommended Method`: using a docker image to compile out the executable binary and then load it, please refer [recommended dockerfile](Dockerfile),
for `go.mod` and `go.sum` are in the root directory, so execute the below command in the root directory `growing-into-an-excellent-golang-developer`
  + 
    ```shell
    sudo docker build -t chinlying/grpc-hello:V1.0 -f ./project-layout/services/hello/Dockerfile .
    ```
    + Advantage: Compilation does not depend on the local environment, it's suitable for cross platform
  + the compiled image size is only 17.6MB <br>
    ![image size](../../../images/size%20of%20the%20built%20image.png)
  + where the compiled executible binary is 11.97MB
    ```shell
    sudo docker run -it --rm chinlying/grpc-hello:V1.0 ls -l /go/app
    ```
    ![binary size](../../../images/size%20of%20the%20compiled%20executible%20binary.png)

+ put the generated image into a container and run 
```shell
docker run -p 50051:50051 --name grpc-hello chinlying/grpc-hello:V1.0
```

+ testing
```shell
cd client
go build && ./client
```

+ please refer to [common docker commands](../../../docker/commands.md) for more docker operations