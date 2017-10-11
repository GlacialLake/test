# tcp_file_transmitter
This is a TCP-based file transmitter with the function of client authentication.<br>
## Usage
### Server
python3 serv.py \<password\(added quotes\)\> [maximum file size(KB)]
### Client
python3 cli.py \<dest_ip\> \<password\(added quotes\)\> \<filename\>
## Protocol
Both of the server and the valid client know the same password previously.<br>
\(1\)<br>The client connect to the server, and then wait for receiving.<br>
\(2\)<br>The server send a token string to the client (basically it is the time stamp of the server), and then wait for receiving.<br>
\(3\)<br>The client reveive the token, concatenate the token with the password, and then use SHA512 to generate a string cli_hex. Then the client send the string cli_hex+':'+filesize+':'+filename to the server, and wait for receiving.<br>
\(4\)<br>The server receive the long string, authenticate the string and check if the filesize is too large and if the filename is duplicate, and then send a response string to the client. If the sent response is "AC", the server will wait for receiving file from the client (because the file size is known, the server can determine when the file is completely received), then close the TCP connection; otherwise, the server will close the TCP connection directly.<br>
\(5\)<br>If the client receive a "AC", it will send the file to the client and then close the TCP connection and exit. Otherwise, the client will close the TCP connection and exit directly.<br>
### response semantics
"WPW" : wrong password<br>
"DFN" : duplicate file name<br>
"EFS" : exceeded file size<br>
"AC" : accepted
