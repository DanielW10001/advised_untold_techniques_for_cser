# key

- Asymmetric Cryptography: A pair of keys, one for encryption, one for decryption
    - Algorithms: RSA, DSA, ECDSA
    - Secret Transfer:
        - Everyone Can Encrypt, Only Receiver Can Decrypt
        - Encryption Key go public, Decryption Key go private
        - Sender use encryption key to encrypt, Receiver use decryption key to decrypt; Stranger only have public encryption key, cannot decrypt secret.
    - Digital Sign:
        - Everyone Can Decrypt, Only Signer Can Encrypt
        - Decryption Key go public, Encryption Key go private
        - Signer use encryption key to encrypt, Reader use decryption key to decrypt; Stranger only have public decryption key, cannot encrypt data.
- Host Key: A pair of private keys and public keys
    - Private Key: Used for Decryption
    - Public Key: Used for Encryption
    - Generation: `ssh-keygen -b 4096 -C "USER@HOST"`
        - Default Host Key: `ssh-keygen -A`
        - Passphrase: Key to En/Decrypt Private Key
    - Local System Key:
        - POSIX: `/etc/ssh/ssh_host_rsa_key[.pub]`
        - Windows: `%ProgramData%/ssh/ssh_host_rsa_key[.pub]`
        - Must not have passphrase
    - Local User Key: `~/.ssh/id_rsa[.pub]`
- Host Identity
    - Use Host Pubkey to identify computers
    - Known Host: `/etc/ssh/ssh_known_hosts`, `~/.ssh/known_hosts`
    - Pull Key: `ssh-keyscan -H -p PORT HOST >> ~/.ssh/known_hosts`
    - Delete Key: `ssh-keygen -R HOST[:PORT]`
    - Generate Local Host Fingerprint (Hash of Host Pubkey): `ssh-keygen -l -E sha256 -f /etc/ssh/ssh_host_rsa_key.pub`
- Authorizing
    - Use User Pubkey to authorize users
    - Authorized Remote Public Keys: `~/.ssh/authorized_keys`
        - By Default, Admin Group Account's Authorized Keys on Windows: `%ALLUSERSPROFILE%/ssh/administrators_authorized_keys`
    - Push Key: `ssh-copy-id [-i [IDPATH]] [USER@]TARGET`
        - Or: `cat ~/.ssh/id_rsa.pub | ssh ssh://[USER@]TARGET[:PORT] "umask 077; test -d ~/.ssh || mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"`
    - Pull Key: `ssh ssh://[USER@][TARGET][:PORT] "cat ~/.ssh/id_rsa.pub" >> ~/.ssh/authorized_keys`
- `ssh-agent`: Store decrypted private key for Single Sign-On
    - `ssh-agent -s`: Start service and output bash-style env var
    - `ssh-agent -k`: Kill current agent
    - `ssh-add`: Add `~/.ssh/id_ALGO` to running ssh-agent
    - `ssh-add PRIVATEKEY_PATH`: Add private key to running ssh-agent
    - `ssh-add -l`: List current private keys in agent
    - Agent Forwarding: Allow slave to use master's agent to log in

Auto start and add private key in `~/.bashrc`:

```bash
eval `ssh-agent -s`
ssh-add
```
