# Ansible SCM-SAFE-ish certificates generator

Configure your CA name and certificates info in the create-certs.yml. You can configure as many cluster_certs as needed.
Alternate names ( `DNS.0 ... N`, `IP.0 ... N` ) are configured using the `dns_list` and `ip_list` arrays.


``` yaml
vars:
    ca_cn: jenkins-ca
    cluster_certs:
      - { cn: jenkins-master
            , cnf: jenkins-master
            , dns_list: [jenkins-master.local]
            , ip_list: [192.168.99.101] }
    vault_password_file: vault-pwd.ignore.txt
    certs_dir: files/certs
    ca_key: "{{ ca_cn }}-key.pem"
    ca_cert: "{{ ca_cn }}.pem"
```

Generate a file called `vault-pwd.ignore.txt` with the password you want to encrypt 
the generated certificate keys with.

``` shell
$ echo "MyCr4zYM3Ga$tr1nG" > vault-pwd.ignore.txt
```

Then execute the create-certs.yml playbook against the local machine

``` shell
$ ansible-playbook -i local, create-certs.yml
```

This will then generate the certficates and place them under the /files/certs folder. 
The private key files will be automatically encrypted by Ansible Vault using the previoulsy 
configured password file (`vault-pwd.ignore.txt`), making it SAFE-ish to commit them to SCM (better than nothing :-D).