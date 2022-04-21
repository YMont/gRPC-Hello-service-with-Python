# gRPC-Hello-service-with-Python

## Quickly start
**Clone gRPC repository**
```typescript=
$ git clone https://github.com/grpc/grpc.git
```

**Install virtual enviroment**<br />
Using virtualenv allows you to avoid installing Python packages<br />
globally which could break system tools or other projects 
```typescript=
$ python -m pip install virtualenv
```

**Create virtual environment**<br />
virtualenv + folder name
```typescript=
$ virtualenv gRPC_project
```

**Activate virtual environment**<br />
cd your folder, then activate this virtual environment.<br />
Later, if it works, it will be appear parentheses with folder name in front of `D:\path`
```typescript=
$ Scripts\activate
```
![](https://i.imgur.com/8Ulp9cQ.png)

**Install gRPC package**<br />
Install two gRPC package, one is grpcio, another is grpcio-tools.
```typescript=
$ python -m pip install grpcio
$ python -m pip install grpcio-tools
```
If you already install, using command`pip list` to check package is exist.<br />
![](https://i.imgur.com/AmLlrIk.png)

**Server run**<br />
```typescript=
$ python greeter_server.py
```
![](https://i.imgur.com/fzDrgdC.png)

**Client run**<br />
```typescript=
$ python greeter_client.py
```
![](https://i.imgur.com/ObyjQ8X.png)

Congratulations! You finish first step.

---
## Advance update the gRPC service
Here, we will define another method `SayHelloAgain`.<br />
So, first step is that add your requirement in the **proto** file.<br />
Second, generate and update gRPC code.<br />
Third, modify method in Python and update and run the application(Python's server/client code).<br />

**Add requirement**<br />
reference the sample code which is the path `examples/protos/helloworld.proto`<br />
(Before)
```typescript=
// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}
```
(After)(New method`SayHelloAgain`)
```typescript=
// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}
```

**Generate and update gRPC code**<br />
```typescript=
$ python -m grpc_tools.protoc -I../../protos --python_out=. --grpc_python_out=. ../../protos/helloworld.proto
```

**Update and run the application(server/client)**
I modify the application Python code so that I can observe the change.<br />
`server.py`(path:`D:\grpc\examples\python\helloworld\server.py`)<br />
(Before)<br />
```python=
class Greeter(helloworld_pb2_grpc.GreeterServicer):

    def SayHello(self, request, context):
        return helloworld_pb2.HelloReply(message='Hello, %s!' % request.name)
```
(After)<br />
```python=
class Greeter(helloworld_pb2_grpc.GreeterServicer):

    def SayHello(self, request, context):
        return helloworld_pb2.HelloReply(message='Hello, %s!' % request.name)
    
    def SayHelloAgain(self, request, context):
        return helloworld_pb2.HelloReply(message='Hello again, %s!' % request.name)
```

`client.py`(path:`D:\grpc\examples\python\helloworld\client.py`)<br />
(Before)<br />
```python=
def run():
    with grpc.insecure_channel('localhost:50051') as channel:
        stub = helloworld_pb2_grpc.GreeterStub(channel)
        response = stub.SayHello(helloworld_pb2.HelloRequest(name='John'))
        print("Greeter client received: " + response.message)
        response = 
```
(After)<br />
```python=
def run():
    with grpc.insecure_channel('localhost:50051') as channel:
        stub = helloworld_pb2_grpc.GreeterStub(channel)
        response = stub.SayHello(helloworld_pb2.HelloRequest(name='John'))
        print("Greeter client received: " + response.message)
        response = stub.SayHelloAgain(helloworld_pb2.HelloRequest(name='Mary'))
        print("Greeter client received: " + response.message)
```
**Run server/client**<br />
```typescript=
$ python greeter_server.py
$ python greeter_client.py
```
Server state is waiting request.
![](https://i.imgur.com/0QBtmc9.png)

Client send a request to server, and receive a response from server.
![](https://i.imgur.com/kzgnqI1.png)


## Reference

reference:[Installing packages using pip and virtual environments](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)<br />
reference:[gRPC quick start](https://grpc.io/docs/languages/python/quickstart/)<br />
