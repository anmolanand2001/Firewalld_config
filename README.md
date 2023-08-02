# Firewalld_Setup_&_Config

Hello, 
I've been assigned the DevOps task number 4 (Configure Firewalld using nftables on a linux VM).
Setting up a firewalld using nftables in Linux involves several steps. Before proceeding. 

**Firewalld** is a dynamic firewall management tool for Linux operating systems, designed to simplify the process of configuring and managing firewall rules. It is a front-end interface to the underlying firewall frameworks, such as iptables and nftables. It provides a higher-level abstraction that makes it easier for administrators to set up and maintain firewall rules without directly dealing with complex low-level configurations. 

The steps to set up firewalld with nftables on linux distribution are:

1. First of all we need to install firewalld (if not already installed): 
Using the package manager of your Linux distribution to install firewalld.
```bash
sudo apt-get install firewall
```
2. Starting and enabling the firewalld service:
We can use the following commands to start the firewalld service and enable it to start automatically on boot:
```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```
3. Verify that firewalld is active:
We check the status of the firewalld service to ensure it is running without errors:
```bash
sudo systemctl status firewalld
```

4. Set firewalld as the default backend for nftables:
Firewalld can use both iptables and nftables as backends. To ensure it uses nftables, we create the following configuration file:
```bash
sudo nano /etc/firewalld/firewalld.conf
```

Add/Modify the following line to the configuration file:
```bash
FirewallBackend=nftables
```

5. Reload the firewalld configuration: We need to reload to make sure changes are implemented
```bash
sudo firewall-cmd --reload
```

6. We Change the default zone of the firewalld (Here we are changing it to 'internal') using the command:
```bash
sudo firewall-cmd --set-default-zone=internal

sudo firewall-cmd --reload
```

7. We check for the ICMP connection using a ping request:
```shell
Test-NetConnection <ip addr>
```
The ping request will succeed as the default configuartion of the firewall is to allow all ICMP requests.

8. We change the default zone to 'trusted':
```bash
sudo firewalld-cmd --set-default-zone=trusted
```
 
9. For the trusted zone we add ssh service to allow ssh connection:
```bash
sudo firewall-cmd --zone=trusted --add-service=ssh --permanent
```
10. We also add ssh port no 22 to the zone for connection using:
```bash
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```
The --permanent option makes the rule persistent across reboots.

11. We reload to implement the changes.
```bash
sudo firewall-cmd --reload
```
12. We check for the ICMP and SSH connection using a ping request:
```shell
Test-NetConnection <ip addr>
Test-NetConnection <ip_addr> -p 22
```
Both the connection requests should be successful as we have enabled the the ssh service for the trusted zone.

13. We change the default zone to 'public':
```bash
sudo firewalld-cmd --set-default-zone=public
```
14. For the public zone we remove the ICMP connection as well using the following command.
```bash
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" protocol value="icmp" drop'
```
15. We relaod and check for the ICMP connection by sending a ICMP ping
```bash
sudo firewall-cmd --reload
```
```bash
Test-NetConnection <ip_addr>
```
This request should not succeed as our command asks the firewall to drop all icmp packets. 

16. We can also Add new custome zones to our firewalld and define custom rules for these zones as per our requirements. The command to add the zone is:
```bash
sudo firewall-cmd --new-zone=<zone_name>
sudo firewall-cmd --reload
```
The above created zone can work just like any default zones available. 

17.We can adjust other settings as well such as customize firewalld settings as needed using the firewall-cmd command or by modifying the firewalld configuration files in the /etc/firewalld/ directory.
To see a list of active rules and zones, use the following commands:
```bash
sudo firewall-cmd --list-all
sudo firewall-cmd --list-all-zones
```
