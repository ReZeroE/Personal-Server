# Personal Web Server (CS 3214)
This is a personal server coded in **C** and **React.js** to support file transfer, MP4 video playback, web token authantification, and so on. The web server is served over Nginx and has persistent connections as per the HTTP/1.1 protocol. (TLS has not been configured in Nginx due to permission issues on the Univerity's SSH server)

To access to source code, please read [Code Access Request](https://github.com/ReZeroE/Personal-Server#code-access-request).


## Serving Files
Like a traditional web server, this personal server support serving files public and private directories in the server’s file system. Private directory access is gated by the user authentification system. The server returns proper content type headers, based on the served file’s suffix. 

Supports file serves for: `.html`, `.js`, `.css`, `.mp4`, `.png`, `.jpg`, `.txt`/`.csv`, and others.

## User Authentication
The web server supports user authentification through simple Json web tokens (the client must obtain a signature from the server that certifies that the server issued the token). Once authenticated, the server would return a cookie to the client. 


 - Signing mechanism: [HMAC](https://www.okta.com/identity-101/hmac/#:~:text=Hash%2Dbased%20message%20authentication%20code,use%20signatures%20and%20asymmetric%20cryptography.) (previously implemented with private/public key pair using RSA)

The servers uses the "stateless authentification" method. Unlike in traditional schemes in which the server must maintain a session store to remember past actions by a client, the presented token contains proof of past authentication, and thus the server can directly proceed in handling the request if it can validate the token. To counter stateless authentification's downside (difficulty in revoking a user’s access since tokens are issued directly to the client), the the server keeps a revocation lists (in which case a session-like functionality).

## HTML5 Fallback
Modern web applications exploit the History API, which is a feature by which JavaScript code in the client can change the URL that’s displayed in the address bar, making it appear to the user that they have navigated to a new URL when in fact all changes to the page were driven by JavaScript code that was originally loaded. The server supports HTML5 fallback by redirecting the user to a pre-specified homepage.


## Multi-client Support (multithreading)
The server is capable of supporting multiple clients simultaneously through multi-threading by accpeting new clients and process HTTP requests even while HTTP transactions with an already accepted clients are still in progress.

With the multithreading approach, the server imposes a reasonable limit to the number of acceptable clients based on the number of available logical cores on the machine.

## Protocol Independence
Ensuring protocol independence requires avoiding any dependence on a specific protocol in the code. Fortunately, the socket API was designed to support multiple protocols from the beginning as its designers foresaw that protocols and addressing mechanisms would evolve.

The server utilizes Linux's dual-bind feature to support both IPv4 and IPv6 clients through TCP/IP.

## Code Access Request
Due to Honor Code concerns, this program is not publicly visible as of right now. If you would like to access the code for recruitment purposes, please contact me at kevinliu@vt.edu. 
