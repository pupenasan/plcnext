[Розширені функції та функціональні блоки](../README.md) -> [Ethernet](README.md)

# TCP/TLS/UDP Error / Status Codes for Ethernet FBs





The following status and error codes can be provided at the STATUS  output of the Ethernet function blocks TCP_*, TLS_*, TLS_*_2 and UDP_*.  Not all error codes apply to  each function block. 

**Status codes (ERROR output = FALSE):**



| Status code (hex) | Meaning                                                      |
| ----------------- | ------------------------------------------------------------ |
| 0x0000            | Situation is normal (no error).                              |
| 0x8000            | Socket is trying to connect the partner.                     |
| 0x8001            | Server is listening for a client.                            |
| 0x8002            | Server has rejected a client because the IP address and port number do not match. |
| 0x8003            | Not all data could be sent. Remaining data will be sent in the next cycle(s). |
| 0x8004            | Not all data received: Received length < Expected length     |
| 0x8005            | Disallowed attempt to  stop TLS. START_TLS was set from TRUE to FALSE during opened TLS socket  (ACTIVE input = TRUE). (Only valid for TLS_SOCKET_2). |



**Error codes (ERROR output = TRUE):**

| Error code (hex) | Meaning                                                      |
| ---------------- | ------------------------------------------------------------ |
| 0xC001           | Socket creation failed.                                      |
| 0xC002           | IP has wrong format.                                         |
| 0xC100           | Unexpected error during connecting of a client to a server.  |
| 0xC101           | Unexpected error during receive operation.                   |
| 0xC102           | Unexpected error during send operation.                      |
| 0xC103           | Unexpected error during bind operation.                      |
| 0xC104           | Unexpected error during listen operation.                    |
| 0xC105           | Unexpected error during accept operation.                    |
| 0xC150           | The parameterization of the send/receive function blocks is inconsistent with  the relating  socket function block. This is the case when:  The send/receive FB both  require secure transmission/reception of data  (SEND_SECURE/RECEIVE_SECURE input = TRUE), but the socket is not yet  initialized for TLS communication (START_TLS input of the *SOCKET* FB is FALSE). The send/receive FB both  require insecure  transmission/reception of data (SEND_SECURE/RECEIVE_SECURE input =  FALSE), but the socket is already initialized for TLS communication  (START_TLS input of the *SOCKET* FB is TRUE). |
| 0xC151           | An error regarding the  START_TLS input of the *SOCKET* function block has occurred. START_TLS  was set from TRUE to FALSE during opened TLS socket (ACTIVE input =  TRUE). (Only valid for TLS_SOCKET.) |
| 0xC201           | Socket creation failed. There are too many open sockets in the underlying socket provider. |
| 0xC202           | An operation on a non-blocking socket cannot be completed immediately. |
| 0xC204           | The datagram is too long.                                    |
| 0xC205           | Only one use of an address is normally permitted.In case of a TCP or TLS connection, this error code can be emitted when a  rising edge is detected at the ACTIVATE input while the ACTIVE and BUSY  outputs both are still not FALSE (i.e., a new connection is requested  while the previous socket termination is not yet completed). The error  also occurs if the controller is switched to STOP and afterwards to the  RUN state as this terminates established connections.This error  is also emitted if a TCP/TLS server shall listen to several clients. To  avoid this error use the newer FB generation TLS_*_2. |
| 0xC206           | The selected IP address is not valid in this context.        |
| 0xC207           | The connection was aborted by the .NET Framework or the underlying socket provider. |
| 0xC208           | The connection was reset by the remote peer.                 |
| 0xC210           | The application tried to send or receive data, and the socket is not connected. |
| 0xC212           | An internal error occurred with unclear reason. On controllers up to firmware 2019.3 one cause for this error is an empty CipherList. Another known cause is trying to send a UDP datagram to a broadcast address (255.255.255.255) using the UDP_SEND or UDP_SEND_2 function block. In case the  error is thrown for  the UDP_SEND_2 FB, ensure that the   gateway in your IP configuration is set to a valid  address (address  0.0.0.0 is invalid and treated as a missing gateway).  (It is not  required that the network contains a device with the configured gateway  address.) |
| 0xC213           | The remote host is actively refusing a connection. The service is not available on the remote host. |
| 0xC214           | Parameters like CipherList, TrustStoreName and IdentityStoreName are invalid or missing. An invalid port number was specified. |
| 0xC224           | Network is down.                                             |
| 0xC225           | Network unreachable.                                         |
| 0xC226           | Network dropped connection on reset.                         |
| 0xC228           | Запит на надсилання або отримання даних було заборонено, оскільки Socket уже закрито. |
| 0xC229           | The connection attempt timed out, or the connected host has failed to respond. |
| 0xC22A           | The operation failed because the remote host is down.        |
| 0xC22B           | There is no network route to the specified host. The remote host may be down. |
| 0xFFFF           | SSL error occurred.  This can be an authentication error or caused by incorrect date and time settings on the controller. Check the certificates. |
