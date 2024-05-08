## a) Cheatsheet
___
#### Metasploit  Meterpreter:

| **Syntax** | **Description** |
| :--- | :--- |
| sysinfo | Display system information |
| ps | List and display running process |
| kill (PID) | Terminate a running process |
| getuid | Display user ID |
| upload or download | upload or download a file |
| background | Move active session to background |
| bglist | Show background running scripts |
| bgrun | Make a script run in background |
| bgkill | Terminate a background process |
| edit \<FILE name> | Edit a file in vi editor |
| shell | access shell on the target machine |
| migrate \<PID> | Switch to another process |
| ? or Help | Shows all the commands |
| exit or quit | Exit Meterpreter session |
| shutdown or reboot | Restart system |
| channel | Show active channels |
- Lisää: https://github.com/security-cheatsheet/metasploit-cheat-sheet
___

#### msfvenom:

| **Switch** | **Syntax** | **Description** |
| :--- | :--- | :--- |
| -p | –p (Payload option) | Display payload standard options |
| –l | –l ( list type) | List module type i .e payload, encoders |
| –f | –f ( format ) | output format |
| –e | -e (encoder) | Define which encoder to use |
| -a | –a (Architecture or platform | Define which platform to use |
| -s | -s (Space) | Define maximum payload capacity |
| -b | -b (characters) | Define set of characters not to use |
| –i | –i (Number of times) | Define number of times to use encoder |
| -x | -x (File name) | Define a custom file to use as template |
| –o | -o (output) | Save a payload |
| –h | -h | Help |
___

#### nmap

| **Switch** | **Example** | **Description** |
| :--- | :--- | :--- |
| | nmap 192.168.1.1 | Scan a single IP |
| | nmap 192.168.1.1 192.168.2.1 | Scan specific IPs |
| | nmap 192.168.1.1-254 | Scan a range |
| | nmap scanme.nmap.org | Scan a domain |
| | nmap 192.168.1.0/24 | Scan using CIDR notation |
| -iL |	nmap -iL targets.txt | Scan targets from a file |
| -iR | nmap -iR 100 | Scan 100 random hosts |
| -exclude | nmap -exclude 192.168.1.1 | Exclude listed hosts |

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/89ddc2d4-e0ca-4f3d-ab45-1cf0a033542f)
###### https://www.comparitech.com/net-admin/nmap-nessus-cheat-sheet/
___





## b) Review


## Lähteet:
- Metasploit Cheat Sheet: https://www.comparitech.com/net-admin/metasploit-cheat-sheet/
- msfvenom cheat sheet: https://www.stationx.net/metasploit-cheat-sheet/
