# enterprise-ansible
Central Enterprise Ansible Repository

## Playbooks
Some playbooks have a default group, if one is not hard-coded if one is
not specified on the command line.  To specify a group, add a variable
called "group" to the oxtra-vars option to the ansible[-playbook] 
command line. (-e "group=<value>") The value can be a group name or a
system name that is in the inventory file>

### Windows Playbooks
> All windows playbooks now rely on the local user's Kerberos tickets for 
> authentication on the remote server.  SSSd will attempt to keep tickets
> current, by renewing them automatically.  To check if tickets are current, 
> run __klist__ and verify the dates.  To get new tickets, run __kinit__ 
> after logging into the system.  This will ask for the domain
> password and fetch a current ticket, replacing any expired ticket.  Use
> __klist__ to list the user's principal and tickets.
```
prompt> kinit dmaple@GMTI.GBAHN.NET
Password for dmaple@GMTI.GBAHN.NET:

prompt> klist
Ticket cache: FILE:/tmp/krb5cc_149813743
Default principal: dmaple@GMTI.GBAHN.NET

Valid starting       Expires              Service principal
07/18/2017 10:06:54  07/18/2017 20:06:54  krbtgt/GMTI.GBAHN.NET@GMTI.GBAHN.NET
	renew until 07/25/2017 10:06:49
```

 * __win-test.yml__ - Tests access to Windows systems or groups.
 * __win-check-updates.yml__ - Checks for updates to Windows systems.
 * __win-update-critsec.yml__ - Apply Critical and Security updates and reboot the servers when needed.
 * __win-update-critsec-cron.yml__ - Cron version of the Critical and Security update playbook.
 * __win-update-all.yml__ - Apply ALL AVAILABLE UPDATES and reboot, if necessary.
  
### Linux Playbooks
 * __bootstrap-rhel5__ - Runs the bootstrap-rhel5 role, followed by the ansible-client role to prepare an EL5 host for management by Ansible.
 * __ansible-client.yml__ - Runs the ansible-client role to create the ansible user and grant necessary access.
 * __spacewalk-join.yml__ - Runs the epel-repo and spacewalk roles to register a host with Spacewalk.
 * __net-snmp.yml__ - Runs the net-snmp role to install and configure net-snmp for SNMPv3 monitoring by EM7.
 * __rhn_check.yml__ - Runs rhn_check on hosts to get them to check-in with Spacewalk or Satellite.
 * __rhncfg-client.yml__ - Runs rhncfg to list configuration channels and verify configuration files (does not make any changes.)
 * __linux-update.yml__ - Runs the default package manager for the Linux system using the ansible package module that auto-detects the system package manager.

### Cross-platform Playbooks
 * __ping-any.yml__ - Basic test of ansible connectivity to any system, using 
 the right method for that system.
 * __tripwire.yml__ - Install Tripwire Axon Agent on Windows or Linux
 * __tripwire_rm_psk.yml__ - Cleanup the PSK file from a host, if it was put there after the host was already registered.

## Roles

 * __ansible-client__ - Creates the ansible user on all linux systems.  It creates the ansible user and group, if needed, copies the authorized_keys and configures sudors for elevated privileges.
 * __bootstrap-rhel5__ - Used on RedHat and CentOS 5 systems to get them ready for management by ansible.  It removes the python-json package, if installed and installs the python-simplejson package.
 * __epel-repo__ - Update some packages and install the epel repository configuration for yum.
 * __net-snmp__ - Install net-snmp packages as needed, and configure snmpd.conf for SNMPv3 access by EM7.
 * __spacewalk__ - Install spacewalk client packages and register a system with the Spacewalk server.
 * __axon-linux__ - Install, configure and register Tripwire Axon Agent on RHEL/CentOS hosts. (currently 64-bit only)
 * __axon-windows__ - Install, configure and register Tripwire Axon Agent on Windows hosts. (currently 64-bit only)

## Configuration Files

 * __ansible.cfg__ - Local configuration file (overrides /etc/ansible/ansible.cfg)

## Inventory Files (To use, add "-i <filename>" to the ansible[-playbook] command.)

 * __hosts-init__ - Default inventory file (copy to "hosts" to make live)
 * __hosts-gci__ - Inventory of Enterprise systems.
 * __tripwire-hosts.yml__ - Inventory of Tripwire client systems for the Tripwire Axon Agent playbooks

