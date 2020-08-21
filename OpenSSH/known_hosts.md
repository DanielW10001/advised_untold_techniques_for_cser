# known_hosts

- System: `/etc/ssh/ssh_known_hosts`
- User: `~/.ssh/known_hosts`

Format:

```known_hosts
[MARKERS ]HOSTNAMES KEYTYPE KEY COMMENT
```

`MARKERS`: `@cert-authority|@revoked`
`HOSTNAMES`: `PATTERN(,PATTERN)*`
