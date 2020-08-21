# hashlib

```python3
hasher = hashlib.(sha(1|224|256|384|512)|blake2[bs]|md5|sha3_(224|256|384|512)|shake_256)(INIT_BYTES)
hasher.name  # Algorithm name
hasher.block_size  # Hash algorithm block size in bytes

(hasher.update(BYTES))+

hasher.hexdigest()
hasher.digest()
hasher.digest_size  # Size in bytes

hasher.copy()
```
