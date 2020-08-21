# Structure: File Hierarchy Standard

```plain
/  # Filesystem Dir Base (Root)
|-vmlinu[xz]  # Linux Kernel
|-bin  # Essential Command Binaries
|  |-cat  # Concatenate Files to stdout
|  |-chgrp  # Change File Group
|  |-chmod  # Change File Permission
|  |-chown  # Change File Owner
|  |-cp  # Copy File
|  |-date  # Print or Set System Time
|  |-dd  # Convert File
|  |-df  # Print Filesystem Disk Space Usage
|  |-dmesg  # Pirnt or Control Kernel Message Buffer
|  |-echo  # Print Text
|  |-false  # Do Nothing Unsuccessfully
|  |-gzip  # GNU Ziper
|  |-hostname  # Print or Set Host Name
|  |-kill  # Send Signal to Processes
|  |-ln  # Make Links
|  |-login  # Begin Sessions
|  |-ls  # List Dir Contents
|  |-mkdir  # Make Dirs
|  |-mknod  # Make Block or Char Special Files
|  |-more  # Page Through Text
|  |-mount  # Mount Filesystem
|  |-mv  # Move/Rename File
|  |-netstat  # Network Statisticer
|  |-ping  # ICMP Network Tester
|  |-ps  # Print Process Status
|  |-pwd  # Print Current Working Dir
|  |-rm  # Remove File
|  |-rmdir  # Remove Dir
|  |-sed  # Edit Stream
|  |-setserial
|  |-sh  # POSIX Compatible Command Shell
|  |-stty  # Print or Set Terminal Settings
|  |-su  # Switch User
|  |-sync  # Flush Filesystem Buffers
|  |-tar  # Archive Files
|  |-test  # Test File Info
|  |-true  # Do Nothing Successfully
|  |-umount  # Unmount Filesystem
|  |-uname  # Print System Info
|-boot  # Boot Loader Static Files
|-dev  # Devices
|  |-MAKEDEV[.local]  # Make Device
|  |-null  # Black Hole Returning EOF
|  |-tty  # Terminal
|  |-zero  # Black Hole Returning Zero Bytes
|  |-stdout
|  |-stderr
|-etc  # Host Config
|  |-lilo.conf
|  |-opt  # Add-on Packages Config
|  |-X11  # X Window System Config
|  |  |-xorg.con  # X.org Config
|  |  |-Xmodmap  # X11 Keyboard Modification
|  |-exports  # NFS Filesystem Access Control List
|  |-fstab  # Filesystem Static Info
|  |-ftpusers  # FTP Daemon User Access Control List
|  |-gateways  # Routed Gateway List
|  |-gettydefs  # getty Config
|  |-group  # User Group Info
|  |-host.conf  # Resolver Config
|  |-hosts  # Host Name Static Info
|  |-hosts.(allow|deny)  # Host Access Control
|  |-hosts.(equiv|lpd)  # Program Specific Trusted Host List
|  |-inetd.conf  # inetd Config
|  |-inittab  # init Config
|  |-issue  # Pre-Login Message & ID
|  |-ld.so.conf  # Extra Shared Lib Searching Dirs
|  |-motd  # Day File Post-Login Message
|  |-mtab  # Filesystem Dynamic Info
|  |-mtools.conf  # mtools Config
|  |-networks  # Network Name Static Info
|  |-passwd  # Password File
|  |-printcap  # lpd Printer Capability Database
|  |-profile  # Shell Init Config
|  |-protocols  #IP Protocol List
|  |-resolv.conf  # Resolver Config
|  |-rpc  # RPC Protocol List
|  |-securetty  # TTY Access Control for root Login
|  |-services  # Network Service Port Names
|  |-shells  # Login Shell Paths
|  |-syslog.conf  # syslogd Config
|  |-xml  # XML Config
|-home  # User Home
|  |-.CONFIG  # Config File or Dir Containing CONFIG Files
|-lib  # Essential Shared Libs and Kernel Modules
|  |-ld*  # Runtime Linker/Loader
|  |-libc.so.*  # Dynamically-Linked C Lib
|  |-modules  # Loadable Kernel Modules
|-libFORMAT  # Alternate Format Seesntial Shared Libs
|-media  # Removable Media
|  |-cdrecorder  # CD Writer
|  |-cdrom  # CD-ROM Drive
|  |-floppy  # Floppy Drive
|  |-zip  # Zip Drive
|-mnt  # Removable Drive
|-opt  # Add-on Packages
|  |-bin  # Binaries
|  |-doc  # Documentations
|  |-include  # C Headers
|  |-info  # Information
|  |-lib  # Libs
|  |-man  # Manuals
|  |-PACKAGE  # Static Package Objects
|  |  |-bin  # Binaries
|  |  |-share  # Shared Files
|  |  |  |-man  # Manual
|  |-PROVIDER  # Provider Name
|-root  # Root User Home
|-run  # Running Processes' Data
|  |-PROGRAM.pid  # Decimal Process ID
|-sbin  # Essential System Binaries
|  |-ctrlaltdel  # Instant Hard Reboot
|  |-fastboot  # Fast Reboot
|  |-fasthalt  # Fast Stop
|  |-fdisk  # Partition Table Manipulator
|  |-fsck  # Filesystem Checker & Repairer
|  |-fsck.*  # Filesystem Specific Checker & Repairer
|  |-getty
|  |-halt  # Stop
|  |-ifconfig  # Network Interface Configer
|  |-init  # Initial Process; Link to /lib/systemd/systemd
|  |-kbdrate  # Keyboard Report Rate
|  |-ldconfig
|  |-mkfs  # Filesystem Builder
|  |-mkfs.*  # Filesystem Specific Builder
|  |-mkswap  # Swap Area Builder
|  |-reboot
|  |-route  # IP Routing Table Controler
|  |-shutdown
|  |-sln  # Statically Link
|  |-ssync  # Statically Flush Filesystem Buffers
|  |-swap(on|off)  # Paging and Swapping En/Disabler
|-sys  # Kernel & Essential System Info
|  |-update  # Filesystem Buffer Flush Daemon
|-srv  # System Services' Data
|-tmp  # Temporary Files
|-usr  # Read-Only Secondary Dir Base other than /
|  |-bin  # User Command Binaries
|  |  |-python3  # Python Interpreter
|  |  |-expect  # Interactive Dialog
|  |-lib  # Libraries
|  |-local  # Local Dir Base
|  |  |-bin  # Binaries
|  |  |-etc  # Host Config
|  |  |-games  # Games & Educational Binaries
|  |  |-include  # C Headers
|  |  |-lib  # Libraries
|  |  |-man  # Manuals
|  |  |-sbin  # System Binaries
|  |  |-share  # Architecture-Independent Dir Base
|  |  |-src  # Source Code
|  |-sbin  # Non-Essential System Binaries
|  |-share  # Architecture-Independent Data
|  |  |-man  # Manuals
|  |  |  |-LOCALE
|  |  |  |  |-manSECTION  # 1: User Programs; 2: System Calls; 3: Library; 4: Special Files; 5: File Formats; 6: Games, Demos; 7: Miscellaneous; 8: System Admin Programs
|  |  |  |  |  |-ARCH
|  |  |-misc  # Miscellaneous Data
|  |  |  |-ascii  # ASCII Char Set Table
|  |  |  |-termcap[.db]  # Terminal Capability Database
|  |  |-color  # Color Management Info
|  |  |  |-icc  # ICC Color Profiles
|  |  |-dict  # Dictionary
|  |  |  |-words  # English Word List
|  |  |-doc  # Miscellaneous Documentation
|  |  |-games  # Static Game Data
|  |  |-info  # GNU Info System
|  |  |-locale  # Locale Info
|  |  |-nls  # Native Language Support Message Dictionary
|  |  |-ppd  # Printer Definitions
|  |  |-sgml  # SGML Data, DTDs
|  |  |  |-docbook
|  |  |  |-tei
|  |  |  |-html
|  |  |  |-mathml
|  |  |-terminfo  # Terminal Info Database
|  |  |-xml  # XML Data, DTDs
|  |  |  |-docbook
|  |  |  |-xhtml
|  |  |  |-mathml
|  |  |-zoneinfo  # Timezone Info & Config
|  |  |-texmf
|  |  |-kbd  # Keyboard
|  |  |-syscons
|  |-games  # Games & Educational Binaries
|  |-include  # Standard C Headers
|  |  |-asm
|  |  |-linux
|  |  |  |-include
|  |  |-bsd  # BSD Headers
|  |-libexec  # Program-Invoking Binaries
|  |-libFORMAT  # Alternate Filesystem Format Libs
|  |-src  # Source Code
|  |  |-linux
|-var  # Variable Data
|  |-cache
|  |  |-fonts  # Locally-Gened Fonts
|  |  |-man  # Locally-Formatted Manuals
|  |  |-www  # WWW Proxy/Cache Data
|  |-lib  # State Info
|  |  |-color  # Color Management
|  |  |-hwclock
|  |  |-misc  # Miscellaneous
|  |  |-xdm  # X Display Manager
|  |  |-EDITOR  # Text Editor Backup & State
|  |  |-PKGTOOL  # Packaging Support Files
|  |  |-PACKAGE
|  |-local  # Local Data
|  |-lock  # Device Lock Files
|  |-log
|  |  |-lastlog  # Last Login
|  |  |-messages  # syslogd Messages
|  |  |-wtmp  # All logins & logouts
|  |-opt  # Add-on Package Data
|  |-run  # Running Process' Data
|  |  |-utmp  # Current Logined User
|  |-spool  # Application Spool Data
|  |  |-cron  # cron & at
|  |  |-lpd  # Printer
|  |  |  |-printer
|  |  |-mqueue  # Outgoing Mail Queue
|  |  |-news
|  |  |-rwho  # rwhod Data
|  |  |-uucp
|  |-tmp  # Preserved Termporary Data
|  |-account  # Process Accounting Logs
|  |-crash  # System Crash Dumps
|  |-games
|  |-mail  # User Mailbox Data
|  |-yp  # Network Info Service Database
```

- `Read-Only`: Read-Only other than Software Installation or Maintenance
- `.CONFIG`: Config file or dir containing `CONFIG` files
