# CURL

Client URL Data Transferring Tool

```bash
curl [options/URLs]
```

[TOC]

## 1. Option

- `-o <FileDir>`: Get and store to `<FileDir>`
- `-O`: Get and store in local file with Remote Name specified in URL
- `-A|--user-agent "USER_AGENT"`: Overriding User Agent
- `-X|--request POST|PUT|DELETE`: Request Method
- `-H|--header "FIELD: VALUE"|@(-|PATH)`: Extra/Overriding Header
- `-d|--data[-binary] DATA|@(-|PATH)`: POST Data
- `-x <Proxy>`, `--proxy <Proxy>`: Use `<Proxy>`; `<Proxy>`: `[protocol://]host[:port]`
- `--noproxy <no-proxy-list>`
    - `<no-proxy-list>`
        - Comma-separated list of no-proxy hosts
        - Use single `*` character to match all hosts and disable proxy
        - Each name is matched as either a domain which contains the hostname, or the hostname itself
            - E.g.: local.com would match local.com, local.com:80, and www.local.com, but not www.notlocal.com

```bash
# Using Here Doc
curl -H|--header|-d|--data[-binary] @- << EOF
DATA
EOF
```

## 2. URL

`PROTOCOL://DOMAIN/PATH`

- Quote URL in double quotes to avoid shell to interfer with it

### 2.1. Part Set

- `{<Element>,<Element>...}`
    - Will be iterated
    - E.g.: `http://site.{one,two,three}.com`

### 2.2. Sequence

- `[<Start>-<End>[:<Step>]]`
    - Iterate from `<Start>` to `<End>`
    - `<Start>`, `<End>`
        - Can have number and letter: `[1-100]`, `[a-z]`
        - Can have leading zero: `[001-010]`
    - `<Step>`: Number
    - Cannot be nested, but can use several ones in an URL
