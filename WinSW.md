# WinSW

Windows Service Wrapper
https://github.com/winsw/winsw

- Create folder `SERVICE`
- Download [WinSW.exe](https://github.com/winsw/winsw/releases/download/VERSION/WinSW.NET461.exe)
- Rename `WinSW.exe` into `SERVICE.exe`
- Write `SERVICE.xml`
    - [Ref](https://github.com/winsw/winsw/blob/master/doc/xmlConfigFile.md)

```xml
<service>
    <id>ID</id>
    <name>NAME</name>
    <description>DESCRIPTION</description>
    <executable>EXE</executable>

    [<serviceaccount>
        [<domain>DOMAIN</domain>]
        <user>USER</user>
        <password>PASSWORD</password>
        [<allowservicelogon>true</allowservicelogon>]
    </serviceaccount>]
    [<serviceaccount>
        <user>LocalSystem</user>
    </serviceaccount>]
    [<serviceaccount>
        <domain>NT AUTHORITY</domain>
        <user>LocalService</user>
    </serviceaccount>]
    [<serviceaccount>
        <domain>NT AUTHORITY</domain>
        <user>NetworkService</user>
    </serviceaccount>]

    {<onfailure action="restart|reboot|none" [delay="NUM sec|min|hour|day"]/>}
    [<resetfailure>NUM sec|min|hour|day</resetfailure>]

    [<arguments>ARGS</arguments>]
    [<startarguments>ARGS</startarguments>]
    [<workingdirectory>WKDIR</workingdirectory>]
    [<priority>Idle|High|RealTime|BelowNormal|AboveNormal</priority>]
    [<stoptimeout>NUM sec|min|hour|day</stoptimeout>]
    [<stopparentprocessfirst>true</stopparentprocessfirst>]
    [<stopexecutable>EXE</stopexecutable>]
    [<stoparguments>ARGS</stoparguments>]

    [<startmode>Automatic|Manual|Boot|System</startmode>]
    [<delayedAutoStart/>]
    {<depend>ID</depend>}
    [<waithint>NUM sec|min|hour|day</waithint>]
    [<sleeptime>NUM sec|min|hour|day</sleeptime>]
    [<interactive/>]

    [<logpath>LOGDIR</logpath>]
    [<log mode="append|roll|reset|none"/>]
    [<log mode="roll[-by-size]">
        <sizeThreshold>SIZE_IN_KB</sizeThreshold>
        <keepFiles>NUM</keepFiles>
    </log>]
    [<log mode="roll-by-time">
        <keepFiles>NUM</keepFiles>
        <pattern>YYYYMMDD</pattern>
        <autoRollAtTime>HH:MM:SS</autoRollAtTime>
    </log>]
    [<log mode="roll-by-size-time">
        <sizeThreshold>SIZE_IN_KB</sizeThreshold>
        <keepFiles>NUM</keepFiles>
        <pattern>YYYYMMDD</pattern>
        <autoRollAtTime>HH:MM:SS</autoRollAtTime>
    </log>]

    {<env name="NAME" value="VALUE"/>}
    {<download from="URL" to="PATH" [proxy="socks5://127.0.0.1:1080"] [failOnError="true"] [auth="basic|sspi" [unsecureAuth="true"] username="USERNAME" password="PASSWORD"]/>}

    [<beeponshutdown/>]
</service>
```

Following var can be used in `SERVICE.xml` in the form of `%VAR%`:

```cmd
%BASE%  # Parent Dir of SERVICE.exe
```

- Write `SERVICE.copies`

```bash
SRC_PATH>DST_PATH  # Move with overwriting
```

- Place `SERVICE.exe`, `SERVICE.xml` and `SERVICE.copies` in folder `SERVICE`
- `SERVICE.exe install|uninstall`: Install/Uninstall
- `SERVICE.exe start|stop|stopwait|restart`: Start/Stop/Stop and wait until stopped/Restart
- `SERVICE.exe status`: `NonExistent`, `Started` or `Stopped`
- `stdout` and `stderr` of service process will be logged into `LOGDIR/SERVICE[.NUM|DATE].out.log` and `LOGDIR/SERVICE[.NUM|DATE].err.log`
- Execute `%WINSW_EXECUTABLE% restart!` command in subprocess to restart service
