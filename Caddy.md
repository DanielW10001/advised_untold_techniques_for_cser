# Caddy

System Wide Config: `/etc/caddy/Caddyfile`
Caddy Home: `/var/lib/caddy`

## 1. CLI

```man
caddy CMD [ARG]...

# Default to Load ./Caddyfile
caddy run [--config PATH] [--adapter caddyfile] [--pidfile PATH] [--environ] [--envfile PATH] [--resume] [--watch]  # Start Caddy in Foreground (Daemon Mode)
caddy start [--config PATH] [--adapter caddyfile] [--pidfile PATH] [--watch]  # Start Caddy in Background
caddy stop [--address ADMIN_ADDRESS]  # Stop Caddy
caddy reload [--config PATH] [--adapter caddyfile] [--address ADMIN_ADDRESS]  # Reload Config

caddy adapt [--config PATH] [--adapter caddyfile] [--pretty] [--validate]  # Adapt Config to JSON
caddy validate [--config PATH] [--adapter caddyfile]  # Validate Config
caddy file-server [--listen ADDRESS] [--browse] [--root PATH] [--domain DOMAIN] [--templates] [--access-log]  # Start File Server; Default: Listen: :80/:443; Root: .
caddy reverse-proxy [--from ADDRESS] --to ADDRESS [--change-host-header]  # Start Reverse Proxy
caddy environ  # Print Env for Debugging
caddy fmt [PATH] [--overwrite]  # Format Caddyfile
caddy hash-password [--plaintext PASSWORD] [--algorithm scrypt] [--salt SALT]  # Hash and Print base64 Password
caddy list-modules [--versions]  # List Installed Modules
caddy trust  # Trust Cert
caddy untrust [--ca ID] [--cert PATH]  # Untrust Cert
caddy [help [CMD]]  # Print Help
caddy version  # Print Version
```

## 2. Config

- Address: `[srv+][tcp/|udp/|unix/|http[s]://]([HOST][:PORT[-PORT]]|PATH)[/PATH]`
    - `*`: `[^.]*`
- `{VAR}`: `[host][port]`, `remote[_(host|port)]`, `http.vars.root`, `dir`, `file`, `header.*`, `labels.*`, `method`, `path[.*]`, `query[.*]`, `re.*.*`, `scheme`, `uri`, `tls_(cipher|version|client_(fingerprint|issuer|serial|subject))`, `env.*`, `system.hostname|slash|os|arch`, `time.now[.common_log|.year]`, `http.error.(status_(code|text)|message|trace|id)`, `http.matchers.file.relative`, `http.regexp.REGEX_NAME.GROUP_(NAME|ID)`, `args.INDEX`
- Duration: `NANOSEC|"NUM(ns|us|ms|s|m|h|d)"`
- Size: `NUM[k|K|m|M|g|G|t|T|p|P][i][b|B]`
- Config Dir:
    - `$XDG_CONFIG_HOME/caddy`
    - Linux: `$HOME/.config/caddy`
    - Windows: `%AppData%/Caddy`
    - macOS: `$HOME/Library/Application Support/Caddy`
    - Android: `$HOME/.config/caddy`

### 2.1. Caddyfile

- Default to `./Caddyfile`

```caddyfile
{  # Global Options
    debug  # Debug Level Log
    http_port PORT  # Internal Only Server Port
    https_port PORT  # Internal Only Server Port
    default_sni SNI
    order DIRECTIVE first|last|(before|after DIRECTIVE)  # HTTP Handler Order
    storage file_system {  # Data Storage
    }
    acme_ca CA_URL
    acme_ca_root PEM_PATH
    email ACME_EMAIL
    admin off|ADDRESS
    on_demand_tls {
        ask ADDRESS
        interval DURATION
        burst COUNT  # Concurrent Request Limit
    }
    local_certs
    key_type ed25519|p256|p384|rsa2048|rsa4096
    auto_https off|disable_redirects
}

(NAME) {  # Reusable Snippet
}

ADDRESS(, ADDRESS)* {
    import NAME [ARG( ARG)*]  # Import Snippet
    import PATH [ARG( ARG)*]  # Import File; * for Any String

    # Defining Named Matcher
    @NAME MATCHER  # One-Liner
    @NAME {  # Conditions in a Single Rule are OR'ed; Rules in a Single Set are AND'ed.
        expression CEL
        file [PATH( PATH)*]  # Default to Try URI Path
        file {
            root PATH( PATH)*
            try_files PATH( PATH)*  # Hit Stored at http.matchers.file.relative
            try_policy first_exist|(smallest|largest)_size|most_recent_modified
            split_path REGEX( REGEX)*
        }
        header FIELD VALUE  # * for Any String
        header_regexp [REGEX_NAME] FIELD REGEX  # Captured to http.regexp.REGEX_NAME.GROUP_(NAME|ID)
        host HOST( HOST)*
        method GET|PUT|POST|DELET( GET|PUT|POST|DELETE)*
        not MATCHER  # Negative Matcher
        not {  # Negative Matcher
        }
        path PATH( PATH)*  # * for Any String (Including /)
        path_regexp [REGEX_NAME] REGEX  # Captured to http.regexp.REGEX_NAME.GROUP_(NAME|ID)
        protocol http|https|grpc
        query KEY=VALUE( KEY=VALUE)*  # * for Any String
        remote_ip CIDR( CIDR)*
    }

    # HTTP Handler Directive
    root [MATCHER] PATH  # Set Root Path of Site; Default to .; * Matcher to Disambiguate Path Argument
    header [MATCHER] [[+|-]FIELD [VALUE|REGEX REPLACE]] {
        FIELD REGEX REPLACE
        [+]FIELD VALUE
        -FIELD
        [defer]
    }
    redir [MATCHER] TO_ADDRESS [CODE|temporary|permanent|html]  # Useful Vars: uri
    rewrite [MATCHER] PATH[?QUERY]  # Override Request URI; Useful Vars: path, query
    uri [MATCHER] strip_(prefix|suffix)|replace REGEX [REPLACE [COUNT]]  # URI Replacement
    try_files PATH[?QUERY]( PATH[?QUERY])*  # Rewrite URI to First Hit
    basicauth [MATCHER] [bcrypt|scrypt [REALM]] {
        USER BASE64_PASSWORD [BASE64_SALT]
    }
    request_header [MATCHER] [[+|-]FIELD [VALUE|REGEX REPLACE]]
    encode [MATCHER] zstd|gzip( zstd|gzip)* {
        gzip LEVEL
    }
    templates [MATCHER] {  # Dynamic Response Body
        mime text/(html|plain)( text/(html|plain))*  # MIME Type
        between OPEN_DELIMETER CLOSE_DELIMETER  # Default to {{ and }}
        root PATH
    }
    handle [MATCHER] {  # Handle Exclusively from Other Handle Blocks
    }
    handle_path MATCHER {  # Handle while Stripping Matched Path Prefix
    }
    route [MATCHER] {  # Manually Ordered Directives
    }
    respond [MATCHER] [BODY] [CODE] {  # Static Response
        body BODY
        close
    }
    reverse_proxy [MATCHER] DEST_ADDRESS( DEST_ADDRESS)* {
        # Destination
        to DEST_ADDRESS( DEST_ADDRESS)*
        flush_interval DURATION  # -1 to Disable Buffer
        transport http {
            read_buffer SIZE
            write_buffer SIZE
            dial_timeout DURATION
            tls
            tls_client_auth NAME|(CERT_PATH KEY_PATH)
            tls_insecure_skip_verify
            tls_timeout DURATION
            tls_trusted_ca_certs PEM_PATH( PEM_PATH)*
            tls_server_name SNI
            keepalive off|DURATION
            keepalive_idle_conns COUNT
            compression off
        }
        transport fastcgi {
            root PATH
            split REGEX
            env KEY VALUE
        }

        # Load Balancing
        lb_policy first|(header FIELD)|round_robin|least_conn|ip_hash|uri_hash|(random_choose COUNT)
        lb_try_duration DURATION
        lb_try_interval DURATION

        # Health Checking
        health_path PATH
        health_port PORT
        health_interval DURATION
        health_timeout DURATION
        health_status CODE  # X for Any Digit
        health_body REGEX
        # Passive
        fail_duration DURATION
        max_fails COUNT
        unhealthy_status CODE  # X for Any Digit
        unhealthy_latency DURATION
        unhealthy_request_count COUNT

        # Header
        header_up [+|-]FIELD [VALUE|REGEX REPLACE]
        header_down [+|-]FIELD [VALUE|REGEX REPLACE]
    }
    php_fastcgi [MATCHER] GATEWAY_ADDRESS( GATEWAY_ADDRESS)* {
        split REGEX
        env KEY VALUE
        root PATH
        index FILENAME( FILENAME)*
    }
    file_server [MATCHER] [browse] {
        root PATH
        hide PATH( PATH)*
        index FILENAME( FILENAME)*
        browse [TEMPLATE_PATH]
    }
    acme_server [MATCHER]

    # Other Option
    bind HOST( HOST)*
    log {  # Enable Loggin
        output stdout|discard
        output file PATH {
            roll_disabled
            roll_size SIZE
            roll_keep COUNT
            roll_keep_for DURATION
        }
        output net ADDRESS

        format console|json|logfmt|(single_field FIELD) {
            message_key KEY
            level_key KEY
            time_key KEY
            name_key KEY
            caller_key KEY
            stacktrace_key KEY
            line_ending STRING
            time_format FORMAT
            level_format FORMAT
        }

        level INFO|DEBUG
    }
    handle_errors {
        # HTTP Cat Error Prompt
        rewrite * /{http.error.status_code}
        reverse_proxy https://http.cat
    }
    tls internal|ACME_EMAIL|(CERT_PATH KEY_PATH) {
        protocols MIN [MAX]
        ciphers CIPHER( CIPHER)*
        curves CURVE( CURVE)*
        alpn VALUE( VALUE)*
        load PATH( PATH)*
        ca CA_URL
        ca_root PEM_PATH
        dns PROVIDER [ARG...]
        on_demand
        client_auth {
            mode request|require|verify_if_given|require_and_verify
            trusted_ca_cert BASE64_DER
            trusted_ca_cert_file PATH
            trusted_leaf_cert BASE64_DER
            trusted_leaf_cert_file PATH
        }
    }
}

# COMMENT
"CONTENT"  # String; \" for Quote Literal
`CONTENT`  # Stirng; " for Quote Literal
{VAR}  # Variable
{$ENV_VAR}  # Environment Variable
# Matcher: Match Requests:
*  # Any Requests; Default; Used to Disambiguate Path Argument
/PATH  # * for Any String (Including /)
@NAME  # Named Matcher
```

### 2.2. JSON

```json
{
    "admin": {  // API Endpoint
        "disabled": true,
        "listen": "ADDRESS",
        "enforce_oringin": true,  // Origin Check
        "origins": [
            "ADDRESS"
        ],
        "config": {
            "persist": false  // Write Config to Disk
        }
    },
    "logging": {
        "sink": {  // Unstructured Logs
            "writer": {WRITER}
        },
        "logs": {
            "NAME": {  // "default" to Config Default Log Group
                "writer": {WRITER},
                "encoder": {ENCODER},
                "level": "FATAL|PANIC|ERROR|WARN|INFO|DEBUG",
                "sampling": {
                    "interval": DURATION,
                    "first": COUNT,
                    "thereafter": COUNT
                },
                "include": ["LOGGER"],
                "exclude": ["LOGGER"]
            }
        }
    },
    "storage": {STORAGE},
    "apps": {
        "http": {
            "servers": {
                "NAME": {
                    "listen": ["ADDRESS"],
                    "routes": [{
                        "match": [{
                            "host": ["HOST"]
                        }],
                        "handle": [{
                            "handler": "static_response",
                            "body": "CONTENT"
                        }]
                    }]
                }...
            }
        },
        "tls": {
            "certificates": {
                "automate": [""],
                "load_files": [{
                    "certificate": "CERT_PATH",
                    "key": "KEY_PATH",
                    "format": "pem",
                    "tags": ["TAG"]
                }],
                "load_folders": ["PATH"],
                "load_pem": [{
                    "certificate": "CERT_PATH",
                    "key": "KEY_PATH",
                    "tags": ["TAG"]
                }]
            },
            "automation": {
                "policies": [{
                    "subjects": ["ADDRESS"],
                    "issuer": {
                        "module": "internal",
                        "ca": "ID",
                        "lifetime": DURATION,
                        "sign_with_root": true
                    },
                    "issuer": {
                        "module": "acme",
                        "ca": "URL",
                        "test_ca": "URL",
                        "email": "ACME_EMAIL",
                        "external_account": {
                            "key_id": "ID",
                            "hmac": "HMAC"
                        },
                        "acme_timeout": DURATION,
                        "challenges": {
                            "http": {
                                "disabled": true,
                                "alternate_port": PORT
                            },
                            "tls_alpn": {
                                "disabled": true,
                                "alternate_port": PORT
                            },
                            "dns": {
                                "provider": {
                                    "name": "cloudflare",
                                    "api_token": "TOKEN"
                                },
                                "ttl": DURATION
                            },
                            "bind_host": "ADDRESS"
                        },
                        "trusted_roots_pem_files": ["PATH"]
                    },
                    "must_staple": true,
                    "renewal_window_ratio": DURATION,
                    "key_type": "ed25519|p256|p384|rsa2048|rsa4096",
                    "storage": {
                        "module": "file_system",
                        "root": "PATH"
                    },
                    "on_demand": true
                }],
                "on_demand": {
                    "rate_limit": {
                        "interval": DURATION,
                        "burst": COUNT
                    },
                    "ask": "URL"
                },
                "ocsp_interval": DURATION,
                "renew_interval": DURATION
            },
            "session_tickets": {
                "key_source": {
                    "provider": "standard|distributed",
                    "storage": {
                        "module": "file_system",
                        "root": "PATH"
                    }
                },
                "rotation_interval": DURATION,
                "max_keys": COUNT,
                "disable_rotation": true,
                "disabled": true
            },
            "cache": {
                "capacity": COUNT
            }
        }
    }
}
```

### 2.3. API

```plain
POST /stop  # Stop Caddy
POST /load  # Load Config
GET /config/[PATH]  # Get Config
POST /config/[PATH]  # Set/Append Config
PUT /config/[PATH]  # Create/Insert Config
PATCH /config/[PATH]  # Replace Existing Config
DELETE /config/[PATH]  # Delete Config
```

## 3. Static File Serving

- Data Dir:
    - `$XDG_DATA_HOME/caddy`
    - Linux: `$HOME/.local/share/caddy`
    - Windows: `%AppData%/Caddy`
    - macOS: `$HOME/Library/Application Support/Caddy`
    - Android: `$HOME/caddy`, `/sdcard/caddy`
- Default Index: `ROOT/index.html`
- Cert & Key: `DATA_DIR/certificates/acme-v02.api.letsencrypt.org-directory/DOMAIN/DOMAIN.crt|key`

### 3.1. Template: Go `text/template`

- `{{VAR}}`
- `now`: Current Time
- `date "DAY MONTH DATE HH:MM:SS ZONE YEAR"`
