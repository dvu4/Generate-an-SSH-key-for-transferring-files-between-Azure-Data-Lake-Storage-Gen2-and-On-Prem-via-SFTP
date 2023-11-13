# Generate-an-SSH-key-for-transferring-files-between-Azure-Data-Lake-Storage-Gen2-and-On-Prem-via-SFTP


# SSH Key Setup Guide

## Step 1: Login via SSH

- Open a new terminal
- Type: `ssh user@myserver.myorganization.com`
- Password: `mypassword`
- Change directory: `cd /home/users/user/.ssh/`


## Step 2: Generate SSH key (id_rsa)

- Run: `ssh-keygen -t rsa -b 4096`
- Save SSH key with passphrase (`mypassword`), and store it in Key Vault as `pmafos-key-passphrase`
- Two keys will be created: `id_rsa` and `id_rsa.pub`

`id_rsa` example:

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,02658F6FDCCF632846D715E889E40252

+Z1/Xz3j4tTCgztmpUMd9X9TOxy
...
-----END RSA PRIVATE KEY-----
```


`id_rsa.pub` example:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC+pNQ1M0VQQp3JVcDw== user@myserver.myorganization.com
```

## Step 3: Grant login access to users

- Run: `cat id_rsa.pub >> authorized_keys`
- After adding the public key, `authorized_keys` will be updated.

`authorized_keys` example:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQpR0doZDeD0GfnkuH maghagt5@IT-MDR-AGHAGO.local
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAFumnX3lmw== ctbro@hpll01
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAUcl0Js7 jkapalu2@ECO-MCH-JKAPALU.local
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAeMDz+w35B jkapalu2@aamddsvpf1la001
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABA+0WSfuk= jchensn@MER-LDR-CHENJUS
ssh-rsa AAAAB3NzaC1yc2EAAAADAQAjtWjk= rleezh@ECO-MCH-RYANLEE.local
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDrrXk= jchensn@MER-LDR-CHENJUS
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAbpCT1jE= dvuiw@RPU-DVUIW-9VLH
ssh-r
```

## Step 4: Copy private key `id_rsa` to local machine

- Open a new terminal
- Type: `scp user@myserver.myorganization.com:/home/users/myserver/.ssh/id_rsa /Users/dvuiw/Desktop`
- Enter password: `mypassword`

## Step 5: Save SSH secret key into Key Vault

- Ensure Key Vault `prodfixdspmafoseus2kv02` is created with permission model `Vault access policy`
- Login to Azure CLI: `az login`

- Save base64 version of SSH secret key in Key Vault `prodfixdspmafoseus2kv02` as `pmafos-key`:
  - `az keyvault secret set --name pmafos-key --vault-name prodfixdspmafoseus2kv02 --file id_rsa --description "Save SSH Private Key" --encoding base64`

- Add passphrase `mypassword` into file `ssh_pass_phrase`:
  - `echo mypassword > ssh_pass_phrase`

- Save the passphrase in Key Vault `prodfixdspmafoseus2kv02` as `pmafos-key-passphrase`:
  - `az keyvault secret set --name pmafos-key-passphrase --vault-name prodfixdspmafoseus2kv02 --file ssh_pass_phrase --description "Save SSH Passphrase"`



![Key Vault](./images/adf-keyvault-10.png)
