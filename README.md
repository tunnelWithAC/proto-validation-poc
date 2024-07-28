# proto-validation-poc

* [protoc-gen-validate](https://github.com/bufbuild/protoc-gen-validate) (PGV) is a protoc plugin to generate polyglot message validators. While protocol buffers effectively guarantee the types of structured data, they cannot enforce semantic rules for values. This plugin adds support to protoc-generated code to validate such constraints.
* [proto-gen-validate Python](https://github.com/bufbuild/protoc-gen-validate/tree/main/python) Python implementation
* [Protovalidate](https://github.com/bufbuild/protovalidate/tree/main) is a series of libraries designed to validate Protobuf messages at runtime based on user-defined validation rules. Powered by Google's Common Expression Language (CEL), it provides a flexible and efficient foundation for defining and evaluating custom validation rules. The primary goal of protovalidate is to help developers ensure data consistency and integrity across the network without requiring generated code. `protovalidate` is the spiritual successor to protoc-gen-validate.
* [protovalidate examples](https://github.com/bufbuild/protovalidate/tree/main/examples)
* [Common Expression Language](https://cel.dev/overview/cel-overview)  (CEL) is a general-purpose expression language designed to be fast, portable, and safe to execute. You can use CEL on its own or embed it into a larger product. CEL is a great fit for a wide variety applications, from routing remote procedure calls (RPCs) to defining security policies. CEL is extensible, platform independent, and optimized for compile-once/evaluate-many workflows.
* [cel-spec Github](https://github.com/google/cel-spec) / [CEL intro](https://github.com/google/cel-spec/blob/master/doc/intro.md)
* [protovalidate-net](https://github.com/telus-oss/protovalidate-net) is the C# implementation of protovalidate, designed to validate Protobuf messages at runtime based on user-defined validation constraints. 



```
syntax = "proto3";

package examplepb;

import "validate/validate.proto";

message Person {
  uint64 id = 1 [(validate.rules).uint64.gt = 999];

  string email = 2 [(validate.rules).string.email = true];

  string name = 3 [(validate.rules).string = {
    pattern:   "^[A-Za-z]+( [A-Za-z]+)*$",
    max_bytes: 256,
  }];

  Location home = 4 [(validate.rules).message.required = true];

  message Location {
    double lat = 1 [(validate.rules).double = {gte: -90,  lte: 90}];
    double lng = 2 [(validate.rules).double = {gte: -180, lte: 180}];
  }
}
```
