### `nmap` — Network scanner.

**Default Usage**
	`nmap [HOST]` - Host can be IP address or name 

**OPTIONS**
- `-sV` = Version Scan
- `-sC` = Default Script Scan
- `-sT` = Scan Only TCP
- `-Pn` = Don't Ping
- `-O` = Enable OS detection
- `-T4` = Throttle
- `-p` = Choose which ports to scan (can be range)
- `-p-` = Scan ALL Ports
- `--open` = Scan All Open Ports
- `-A` = Perform ALL possible scans (will take a while) (use with -p- for full scan)

