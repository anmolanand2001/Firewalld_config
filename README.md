# Firewalld_Setup_&_Config

Hello, 
I've been assigned the DevOps task number 4 (Configure Firewalld using nftables on a linux VM).

Setting up a firewalld using nftables in Linux involves several steps. Before proceeding, ensure that your Linux distribution supports nftables and firewalld. Some distributions might still use iptables as the default firewall solution. Here are the steps to set up firewalld with nftables:

1. Install firewalld (if not already installed): 
Use the package manager of your Linux distribution to install firewalld. For example, on Debian/Ubuntu, you can use the following command:
```bash
sudo apt-get install firewall
```
2. Start and enable firewalld service:
Use the following commands to start the firewalld service and enable it to start automatically on boot:
```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```
3. Verify that firewalld is active:
Check the status of the firewalld service to ensure it is running without errors:
```bash
sudo systemctl status firewalld
```

4. Set firewalld as the default backend for nftables:
Firewalld can use both iptables and nftables as backends. To ensure it uses nftables, create the following configuration file (if it doesn't exist):
```bash
sudo nano /etc/firewalld/firewalld.conf
```

Add the following line to the configuration file:
```bash
FirewallBackend=nftables
```

5. Reload the firewalld configuration: After making changes to the configuration, reload the firewalld service for the changes to take effect:
```bash
sudo firewall-cmd --reload
```

6. Configure firewalld rules: Use the firewall-cmd command to manage firewalld rules. For example, to allow incoming SSH traffic, use:
```bash
sudo firewall-cmd --zone=public --add-service=ssh --permanent
```
The --permanent option makes the rule persistent across reboots.


7. Enable the default zone: Firewalld uses zones to group rules with specific network interfaces. By default, firewalld uses the "public" zone. Ensure the default zone is enabled:
```bash
sudo firewall-cmd --set-default-zone=public
```

8. Adjust other settings (optional): You can customize firewalld settings as needed using the firewall-cmd command or by modifying the firewalld configuration files in the /etc/firewalld/ directory. Check the rules and zones:
To see a list of active rules and zones, use the following commands:
```bash
sudo firewall-cmd --list-all
sudo firewall-cmd --list-all-zones
```

Remember that modifying firewalls can have significant security implications, so make sure you understand the impact of your rules before applying them. Always verify your rules and test the firewall to ensure it behaves as expected before relying on it in a production environment.
