# conn_tcpip
ASCII/binary transfer between two remote MATLAB hosts using TCP/IP protocol



  conn_tcpip manages TCP/IP client/server communication
 
_______________________________________________________________________
_COMMANDS:_
                                                   
    conn_tcpip('open','server' [,PORT, PUBLICKEY])   : open TCP/IP port in local machine and waits for client to connect (default PORT=6111 KEY='')
                                                      If no PORT is entered, conn_tcpip will first attempt port 6111 and if that fails it will
                                                      bind instead to the first available port. 
 
    conn_tcpip('open','client' [,IP, PORT, PRIVATEKEY]) : connect to server TCP/IP port at IP:PORT (default IP='127.0.0.1' PORT=6111 KEY='')
                                                      If a non-empty PUBLICKEY string was provided when opening the server, the server will reject 
                                                      any client connection where conn_tcpip('keypair',PRIVATEKEY) is different from PUBLICKEY
                                                      (note: use [PUBLICKEY,PRIVATEKEY]=conn_tcpip('keypair') to generate a one-time-use KEY pair)
 
    conn_tcpip('write',VAR)                         : sends data (from server/client to client/server)
                                                      VAR may be any Matlab variable (numeric/string/cell/struct/etc.)
                                                      returns 1 if success, 0 if error
 
    VAR = conn_tcpip('read')                        : receives data (from server/client to client/server)
 
    conn_tcpip('writefromfile',FILENAME)            : sends raw data from file 
                                                      returns 1 if success, 0 if error
 
    TIMESTAMP = conn_tcpip('readtofile',FILENAME)   : receives raw data and save it to file 
                                                      returns TIMESTAMP (conn_tcpip sethash timestamp) or MD5-hash (conn_tcpip sethash md5) of received data (empty if error)
 
    conn_tcpip('close')                             : stops server/client
    conn_tcpip('clear')                             : clears connection buffers
 
    conn_tcpip('writekeepalive')                    : sends an empty/keepalive packet
    conn_tcpip('sethash','md5')                     : uses MD5-hash to identify changes in files (slower)
    conn_tcpip('sethash','timestamp')               : uses filesystem last-modified timestamps to identify changes in files (faster)
    [publickey, privatekey] = conn_tcpip('keypair') : generates one-time-use public (used by server) & private (used by client) key pair
    publickey = conn_tcpip('keypair', privatekey)   : generates public key (used by server) from user-defined private key (used by client)
 
_______________________________________________________________________
_EXAMPLES:_

ON MACHINE #1 (server)
 
      >> conn_tcpip open server
      Opening port 6111...
      Waiting for client connection...
      *************************************************************************************************************
  
      To connect to this server from a different machine, use the Matlab syntax:
         conn_tcpip open client alfonsosmbp2018.lan
  
      To connect to this server using ssh-tunneling, use the following syntax instead:
         $(OS-command)     :   ssh -L 6111:localhost:6111 alfnie@alfonsosmbp2018.lan
         >(Matlab-command) :   conn_tcpip open client 127.0.0.1 6111
  
      *************************************************************************************************************
 
ON MACHINE #2 (client)
 
      >> conn_tcpip open client alfonsosmbp2018.lan
      Connecting to alfonsosmbp2018.lan:6111...
      Succesfully established connection to server
 
ON MACHINE #1 or #2
    
      >> conn_tcpip('write', rand(1,10));
 
ON MACHINE #2 or #1
    
      >> disp(conn_tcpip('read'));
      0.3902    0.8448    0.3720    0.7076    0.3457    0.8772    0.7505    0.4344    0.4007    0.3729
      
      
SEE ALSO: CONN (www.conn-toolbox.org)

_______________________________________________________________________
                                                   Copyright & licencing
                                                   
                                                   
This software is distributed under the MIT/X Consortium License

Copyright (c) 2009

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.


