# Compilar .proto a golang

```protobuf
syntax = "proto3";
package tutorial;

import "google/protobuf/timestamp.proto";

option go_package = "local/prueba";

message Person {
    string name = 1;
    int32 id = 2;  // Unique ID number for this person.
    string email = 3;
  
    enum PhoneType {
      MOBILE = 0;
      HOME = 1;
      WORK = 2;
    }
  
    message PhoneNumber {
      string number = 1;
      PhoneType type = 2;
    }
  
    repeated PhoneNumber phones = 4;
  
    google.protobuf.Timestamp last_updated = 5;
  }
  
  // Our address book file is just one of these.
  message AddressBook {
    repeated Person people = 1;
  }
```

Compilar protobuffers

```bash
protoc -I=. --go_out=./ ./archivo.proto
```

```bash
protoc -I=$SRC_DIR --go_out=$SRC_DIR  $SRC_DIR/hello.proto
```

Compilar servidor gRPC

```bash
protoc --go_out=$SRC_DIR --go_opt=paths=source_relative --go-grpc_out=$SRC_DIR --go-grpc_opt=paths=source_relative proto/hello.proto
```

