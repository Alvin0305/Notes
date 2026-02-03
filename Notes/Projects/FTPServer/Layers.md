### Application Layer
- FTP Logic
- Auth
- GET, PUT
- Files
### Protocol Layer
- Headers
- Framing
- Size
### Transport
- Socket
- send
- recv
### Kernel
- Kernel 

#### Flow
- Server in Application layer calls `receiveRequest` in Protocol Layer
- `receiveRequest` in Protocol Layer don't know how much size is the request or anything
- `receiveRequest` in Protocol Layer calls `receiveHeader` in Transport Layer
- `receiveHeader` with a given buffer size receive until it gets a `\n\n` and put the extra in a vector. And returns the header to `receiveRequest`
- `receiveRequest` then call `receiveBody` in Transport layer with the `payloadSize` and the `writer` in hand. 
- `receiveBody` will take the extra from the vector and will recv from the socket and streams it into the `writer` and return backs to `receiveRequest`
- `receiveRequest` now has the request in hand. Handles it in the application layer
- Application layer generates the required response to be sent back and passes it to `sendRequest` in protocol layer
- Protocol layer will call `sendHeader` in transport layer to send the header and `sendBody` in transport layer to send the body with appropriate `reader`
- 