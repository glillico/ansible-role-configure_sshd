# Ansible Role: Configure SSHD

This role modifies the SSHD configuration on RedHat and Debian-based linux systems to be more secure.

**IMPORTANT**:
The security of the server you are running this on is `YOUR` responsibility. `DO NOT` rely on this role to make your server secure.

I would also like to acknowledge that this ansible role was heavily influenced by Jeff Geerlings [Security](https://galaxy.ansible.com/geerlingguy/security) role.

## Requirements

The `ssh` package must be installed if you want to manage the ssh configuration with this role.

## Role Variables

The available variables are listed below:

### defaults/main.yml

    configure_sshd_Port: 22

The port number that sshd listens on.

    configure_sshd_ListenAddress: 0.0.0.0

The IPv4 address that the sshd service will listen on. By default this will listen on all configured IPv4 addresses.

    configure_sshd_Protocol: 2

The protocol versions sshd supports.

    configure_sshd_PasswordAuthentication: "no"

If Password authentication is allowed.

    configure_sshd_PermitRootLogin: "no"

Whether root can log in using ssh.

    configure_sshd_UseDNS: "no"

Should DNS be used to look up the remote host name, also checks that the DNS hostname matches the ipaddress of the source server.

    configure_sshd_PermitEmptyPasswords: "no"

Can empty password be used.

    configure_sshd_ChallengeResponseAuthentication: "no"

Is challenge-response authentication is allowed.

    configure_sshd_GSSAPIAuthentication: "no"

Is authentication based on GSSAPI is allowed. This only applies when protocol version 2 is being used.

    configure_sshd_X11Forwarding: "no"

Is X11 forwarding is permitted.

    configure_sshd_MaxAuthTries: "3"

What is the maximum number of allowed authentions attempts, also once half this number is reached then further failures are logged.

    configure_sshd_Banner: "/etc/issue"

Send the contents of the specified file to the user before authentication is atempted.

    configure_sshd_AuthorizedKeysFile: "{{configure_sshd_AuthorizedKeysFile_dir}}/%u/authorized_keys"

This file contains the public keys that can be used for user authentication.  This setting uses the value specified by configure_sshd_AuthorizedKeysFile_dir.

    configure_sshd_PubkeyAuthentication: "yes"

Is public key authentication is allowed.

    configure_sshd_Ciphers: "chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr"

The ciphers allowed when protocol version 2 is being used.

    configure_sshd_KexAlgorithms: "curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256"

The available KEX (Key Exchange) algorithms.

    configure_sshd_KexAlgorithms_73: "{{ configure_sshd_KexAlgorithms }},diffie-hellman-group14-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512"

The above KexAlgorithms will be used if the server is running OpenSSH 7.3 or later.

    configure_sshd_MACs: "hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com"

The available MAC (message authentication code) algorithms.

    configure_sshd_DenyUsers: "root"

A space seperated list of usernames that are not allowed to login to this server.  Even if they below to the group defined by configure_sshd_AllowGroups.

    configure_sshd_AllowGroups: "ssh-allowed"

Only users who are members of this group can login to the server.

    configure_sshd_AuthorizedKeysFile_dir: "/etc/ssh/authorized-keys"

The directoy to store a users the authorized keys file in.

    configure_sshd_AuthorizedKeysFile_create_dir: true

Whether the directory defined by `configure_sshd_AuthorizedKeysFile_dir` should be created or not.

    configure_sshd_AllowGroups_create_group: true

Specifies whether the group defined by `configure_sshd_AllowGroups` should be created or not.

### vars/RedHat.yml

    __configure_sshd_name: sshd

The name of the ssh service on RedHat based systems.  This value is used to define configure_sshd_name in the tasks/main.yml file.  This value can be overridden by setting configure_sshd_name in the playbook.

### vars/Debian.yml

    __configure_sshd_name: ssh

The name of the ssh service on Debian based systems.  This value is used to define configure_sshd_name in the tasks/main.yml file.  This value can be overridden by setting configure_sshd_name in the playbook.

### vars/main.yml

    configure_sshd_config_path: /etc/ssh/sshd_config

The full path and filename of the sshd configuration file.

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

This role was created in 2020 by Graham Lillico.
