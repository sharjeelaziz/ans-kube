# Initial networking setup for PIs
The playbook and configuration in this directory will automatically perform all the required networking configuration for your cluster.

To run the playbook:

  1. Update  `inventory`, and list all your Raspberry Pi's _current_ IP addresses under `[pis]`
  2. Update `vars.yml`, and make sure each Pi's MAC address is mapped to the desired final IP addresses and hostnames. Running nmap with sudo would list the MAC addresses of hosts on the network. Run this command `sudo nmap -sP 192.168.86.0/24` with your subnet to find MAC addresses on your network.
  3. Run `ansible-playbook -i inventory main.yml`.

> _Note_: If you don't have your SSH key installed on all the Pis yet, you will also need to pass `-k` to the above command and enter your SSH password (the default for Raspbian is `raspberry`).

After running the playbook the Pis should switch over to their new IP addresses quickly; if they don't, you can forcefully reboot them with the command:

    $ ansible all -i inventory -m shell -a "sleep 1s; shutdown -r now" -b -B 60 -P 0

## Updating hostnames or IPs

Change the IP addresses inside the `inventory` file to the active IP addresses of your Pis and run the playbook again, then reboot all your Pis again.

### References:
[Setup files for dramble](https://github.com/geerlingguy/raspberry-pi-dramble/tree/master/setup)
