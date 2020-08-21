# ssh-keygen

`ssh-keygen [OPTION]...`

```bash
ssh-keygen -b 4096 -C "USER@HOST"  # Gen key pair
ssh-keygen -l[v] [-E md5] -f PUBKEY  # Print Fingerprint
```

- `OPTION`
    - `-b BITS`: Key Length
    - `-C COMMENT`: Key Comment
    - `-F [HOST][:PORT]`: List known hosts
    - `-l`: Print pubkey fingerprint
    - `-E md5`: Set fingerprint algorithm to `md5` instead of default `sha256`
    - `-v`: Verbose
    - `-f KEY`: Specify Key
    - `-t dsa|ecdsa|ecdsa-sk|ed25519|ed25519-sk|rsa`: Specify Key Algorithm, default to `rsa`
