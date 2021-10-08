# Ansible Role : configure_sshd

[![CI](https://github.com/glillico/ansible-role-configure_sshd/workflows/CI/badge.svg)](https://github.com/glillico/ansible-role-configure_sshd/actions?query=workflow%3ACI)

This role modifies the SSHD configuration on RedHat and Debian-based linux systems to be more secure.  A backup of the sshd_config file is taken prior to any change being made.

**IMPORTANT**:
The security of the server you are running this on is `YOUR` responsibility. `DO NOT` rely on this role to make your server secure.

I would also like to acknowledge that this ansible role was heavily influenced by Jeff Geerlings [Security](https://galaxy.ansible.com/geerlingguy/security) role.

## Requirements

The `ssh` package must be installed if you want to manage the ssh configuration with this role.

## Role Variables

### defaults/main.yml

|Variable|Description|
|---|:---|
|configure_sshd_port|The port number that sshd listens on.|
|configure_sshd_listenaddress|The IPv4 address that the sshd service will listen on. By default this will listen on all configured IPv4 addresses.|
|configure_sshd_protocol|The protocol versions sshd supports.|
|configure_sshd_passwordauthentication|If Password authentication is allowed.|
|configure_sshd_permitrootlogin|Whether root can log in using ssh.|
|configure_sshd_usedns|Should DNS be used to look up the remote host name, also checks that the DNS hostname matches the ipaddress of the source server.|
|configure_sshd_permitemptypasswords|Can empty password be used.|
|configure_sshd_challengeresponseauthentication|Is challenge-response authentication is allowed.|
|configure_sshd_gssapiauthentication|Is authentication based on GSSAPI is allowed. This only applies when protocol version 2 is being used.|
|configure_sshd_x11forwarding|Is X11 forwarding is permitted.|
|configure_sshd_maxauthtries|What is the maximum number of allowed authentions attempts, also once half this number is reached then further failures are logged.|
|configure_sshd_banner|Send the contents of the specified file to the user before authentication is atempted.|
|configure_sshd_authorizedkeysfile|This file contains the public keys that can be used for user authentication.  This setting uses the value specified by configure_sshd_authorizedkeysfile_dir.|
|configure_sshd_pubkeyauthentication|Is public key authentication is allowed.|
|configure_sshd_ciphers|The ciphers allowed when protocol version 2 is being used.<br><br>chacha20-poly1305@<span>openssh.com</span><br>aes256-gcm@<span>openssh.com</span><br>aes128-gcm@<span>openssh.com</span><br>aes256-ctr<br>aes192-ctr<br>aes128-ctr|
|configure_sshd_kexalgorithms|The available KEX (Key Exchange) algorithms.<br><br>curve25519-sha256@<span>libssh.org</span><br>diffie-hellman-group-exchange-sha256|
|configure_sshd_macs|The available MAC (message authentication code) algorithms.<br><br>hmac-sha2-512-etm@<span>openssh.com</span><br>hmac-sha2-256-etm@<span>openssh.com</span><br>umac-128-etm@<span>openssh.com</span><br>hmac-sha2-512<br>hmac-sha2-256<br>umac-128@<span>openssh.com</span>|
|configure_sshd_denyusers|A space seperated list of usernames that are not allowed to login to this server.  Even if they belong to the group defined by `configure_sshd_allowgroups`.|
|configure_sshd_allowgroups|Only users who are members of this group can login to the server.|
|configure_sshd_authorizedkeysfile_dir|The directoy to store a users the authorized keys file in.|
|configure_sshd_authorizedkeysfile_create_dir|Whether the directory defined by `configure_sshd_authorizedkeysfile_dir` should be created or not.|
|configure_sshd_allowgroups_create_group|Specifies whether the group defined by `configure_sshd_allowgroups` should be created or not.|
|configure_sshd_allowgroups_user|Specifies the user who should be made a member of the group defined by `configure_sshd_allowgroups`.|

The below KexAlgorithms will be used in addition to the ones defined above if the server is running OpenSSH 7.3 or later.

|Variable|Description|
|---|:---|
|configure_sshd_kexalgorithms_73|The KEX (Key Exchange) algorithms that will be used in addition to the ones defined above if the server is running OpenSSH 7.3 or later.<br><br>diffie-hellman-group14-sha256<br>diffie-hellman-group16-sha512<br>diffie-hellman-group18-sha512|

### vars/RedHat.yml

|Variable|Description|
|---|:---|
|__configure_sshd_name|The name of the ssh service on RedHat based systems.  This value is used to define configure_sshd_name in the tasks/main.yml file.<br>This value can be overridden by setting configure_sshd_name in the playbook.
|__configure_sshd_syslogfacility|Sets the rsyslog facility on RedHat based systems.<br>By default this will be AUTHPRIV.INFO.<br>This value can be overridden by setting configure_sshd_SyslogFacility in the playbook.|
|__configure_sshd_sftpserver|Sets the path of the sftp-server used by the sftp subsystem.<br>This value can be overridden by setting onfigure_sshd_sftpserver in the playbook.|

### vars/Debian.yml

|Variable|Description|
|---|:---|
|__configure_sshd_name|The name of the ssh service on Debian based systems.  This value is used to define `configure_sshd_name` in the tasks/main.yml file.<br>This value can be overridden by setting `configure_sshd_name` in the playbook.|
|__configure_sshd_SyslogFacility|Sets the rsyslog facility on Debian based systems.<br>By default this will be AUTH.INFO.<br>This value can be overridden by setting `configure_sshd_syslogfacility` in the playbook.|
|__configure_sshd_sftpserver|Sets the path of the sftp-server used by the sftp subsystem.<br>This value can be overridden by setting `configure_sshd_sftpserver` in the playbook.|

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - glillico.configure_sshd

## License

MIT

## Author Information

Created in 2020 by Graham Lillico. (Inspired by [geerlingguy/ansible-role-security](https://github.com/geerlingguy/ansible-role-security))
