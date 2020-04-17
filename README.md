# How to Run this Repro

`google.protobuf.Timestamp time = 5` has been added to the Movie proto message. Which running the mesh this attribute is not present in the schema.

## start the gRPC server
`yarn grpc:start`

## start the GraphQL server with node inspect
`node --inspect-brk node_modules/@graphql-mesh/cli/index.cjs.js serve`

## open Chrome inspect
`chrome://inspect/`

## set a breakpoint in getPackageProtoDefinition
set a breakpoint after protobuf load in grpc-with-google-proto/node_modules/@graphql-mesh/grpc/index.cjs.js

## resume script execution
when the above breakpoint is hit observe that `protoDefinitionObject` includes `nested.io` and `nested.google`

## resume script execution
Now the server should be loaded, check the playground and observe that the `time` attribute added to the proto is not queriable in the schema. This is because it has been removed in the `heal` process. The removal happens because when the schema types are first generated via the `getGraphqlQueriesFromProtoService`, the type for `time` is `undefined`, because `google.protobuf.Timestamp` type definition has been lost in the processing of the proto object in `getFromObject`.
```