# Virtual Private Network (VPN)

VPN allows virtually extending a private network across one or multiple other networks which are either untrusted (not controlled by the entity aiming to implement the VPN) or need to be isolated (making the VPN-using site not reach other networks).

## Types

- **Site-to-site**: allows a remote site's network to connect to the main one and be seen as a local network segment. Concentrators on both ends will manage the connection.
- **Remote-access or host-to-site**: allows select remote users to connect to the local network. Concentrator on the local network will manage the connection, whereas the remote system uses special VPN client software.
- **Host-to-host (SSL VPN)**: allows a secure connection between two systems without VPN client software. A VPN concentrator on the local network manages the connections, whereas the host seeking to connect does so using a browser that supports SSL or TSL. 
