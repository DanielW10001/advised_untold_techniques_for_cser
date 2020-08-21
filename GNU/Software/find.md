# find: Search For File

```man
find [OPTION]... [PATH]... [EXPRESSION]

Path: Default to .

Expression: Tests & Actions; Default to -print
  Test:
    # Numeric Argument: NUM: Exactly NUM; -NUM: Less Than NUM, Exclusive; +NUM: Greater Than NUM, Exclusive
    -amin [-|+]NUM  # Last Accessed [-|+]NUM Minutes Ago
    -anewer FILE  # Last Accessed Time is more recent than Last Data Modification Time of FILE
    -atime [-|+]NUM  # Last Accesed [-|+]NUM Days Ago
    -cmin [-|+]NUM  # Last Meta Data Modification [-|+]NUM Minutes Ago
    -cnewer FILE  # Last Meta Data Modification Time is more recent than Last Data Modification Time of FILE
    -ctime [-|+]NUM  # Last Meta Data Modification [-|+]NUM Days Ago
    -empty  # File Data is Empty
    -executable
    -false
    -fstype ext4|btrfs|ufs|nfs|tmp|mfs|4.(2|3)|S5(1|2)K  # File System Type
    -gid [-|+]NUM  # Group ID
    -group NAME|ID
    -ilname "GLOB"  # Symbolic Link with GLOB Content Ignoring Case
    -iname "GLOB"  # Base Name Ignoring Case
    -inum [-|+]NUM  # inode Number
    -ipath|-iwholename "GLOB"  # Path Ignoring Case
    -iregex "REGEX"  # Path Whole Matching REGEX Ignoring Case; -regextype
    -links [-|+]NUM  # Hard Link Count
    -lname "GLOB"  # Symbolic Link with GLOB Content
    -mmin [-|+]NUM  # Last Data Modification [-|+]NUM Minutes Ago
    -mtime [-|+]NUM  # Last Data Modification [-|+]NUM Days Ago
    -name "GLOB"  # Base Name
    -newer FILE  # Last Data Modification Time is more recent than Last Data Modification Time of FILE
    -newer[aBcmt][aBcmt] FILE  # Timestamp is newer than FILE's Timestamp
      # a: Access Time
      # B: Birth Time
      # c: Meta Data Change (Modification) Time
      # m: Data Modification Time
      # t: File Content as a Time
    -nogroup  # No Group corresponds to file's numeric Group ID
    -nouser  # No User corresponds to file's numeric User ID
    -path|-wholename "GLOB"
    -perm [-|/]MODE  # Permission Mode
      # Mode:
      #   Octal: XXX
      #   Symbolic: [ugoa]+[=+][rwx]+(,[ugoa]+[=+][rwx]+)*
      # Default: Exactly: Specified Permission Must Be Set, Others Must NOT Be Set
      # -: At Least: Specified Permission Must Be Set
      # /: At Least Somebody: Somebody's Specified Permission Must Be Set
    -readable  # Permissionly Readable
    -regex "REGEX"  # Path Whole Matching REGEX; -regextype
    -samefile FILE  # The Same inode as FILE
    -size [-|+]NUM[cwbkMG]  # Data Size; b: 512-Bytes Block (Default); c: Bytes; w: 2-Bytes Word; k: KiB; M: MiB; G: GiB
    -true
    -[x]type b|c|d|p|f|l|s|D(,b|c|d|p|f|l|s|D)*  # File Type
      # x: Symbolic Link Alternative
      # b: Block/Buffer Special
      # c: Character/Unbuffered Special
      # d: Directory
      # p: Named PIPE/FIFO
      # f: File
      # l: Symbolic Link
      # s: Socket
      # D: Door
    -uid [-|+]NUM  # User ID
    -used [-|+]NUM  # Last Accessed NUM Days After Last Meda Data Modification
    -user NAME|ID  # User
    -writable  # Permissionly Writable
    -context "GLOB"  # Security Context
  Action:
    -delete  # Implies -depth; -ignore_readdir_race to Tolarence Error
    -exec[dir] "CMD";  # {}: File Name; Default Working Directory is the Starting Directory; dir to Set Working Diretory to File's Directory
    -exec[dir] "CMD {}"+  # xargs CMD; Default Working Directory is the Starting Directory; dir to Set Working Diretory to File's Directory
    -fls FILE  # ls -dils > FILE; /dev/std(out|err) for std(out|err)
    -fprint[0] FILE  # echo 'PATH\n' > FILE; /dev/std(out|err) for std(out|err); 0 for \0 instead of \n
    -fprintf FILE FORMAT  # echo 'CONTENT' > FILE
      # \[abcfnrtv0]
      # \XXX: Octal ASCII Character
      # %a: Last Access Time
      # %AFORMAT: Formatted Last Access Time
      # %b: Disk Usage in 512-Bytes Block
      # %c: Last Meta Data Modification (Change) Time
      # %CFORMAT: Formatted Last Data Modification (Change) Time
      # %d: Directory Tree Depth
      # %D: TODO
      # \\, %%: Literal
      # /dev/std(out|err) for std(out|err)
      # Time Format:
      #   @: Seconds Since Jan 1, 1970, 00:00 UTC
      #   Time:
      #     H, I, k, l: Hour: 00..23, 01..12,  0..23,  1..12
      #     M: Minute
      #     S: Second: XX.XX
      #     p: AM/PM
      #     r: Time: HH:MM:SS [AP]M
      #     T: Time: HH:MM:SS.XX
      #     X: Locale Time Representation: H:M:S
      #     Z: Time Zone
      #     +: Date and Time: YYYY-MM-DD+HH:MM:SS.XX
      #   Date:
      #     a: Weekday Name: Sun..Sat
      #     A: Full Weekday Name: Sunday..Saturday
      #     w: Day of Week: 0..6
      #     U: Week of Year (Sunday as First Day): 00..53
      #     W: Week of Year (Monday as First Day): 00..53
      #     d: Day of Month: 01..31
      #     b, h: Month Name: Jan..Dec
      #     B: Full Month Name: January..December
      #     m: Month: 01..12
      #     j: Day of Year: 001..366
      #     y: Year: 00..99
      #     Y: Year: 1970..
      #     D: Date: MM/DD/YY
      #     x: Locale Date: MM/DD/YY
      #     c: Date and Time: Wek Mon DD HH:MM:SS ZON YYYY
    -print  # Print File Name
  Option:
    Global:  # Affects All Tests and Actions
      -depth|-d  # Depth First Search
      -[-]help  # CLI Help
      -[no]ignore_readdir_race
      -maxdepth DEPTH  # Ignore Files with Depth more then DEPTH
      -mindepth DEPTH  # Ignore Files with Depth less then DEPTH
      -xdev|-mount  # Ignore Other Filesystems
      -noleaf  # Do NOT Assume Files to be Leaf with UNIX Filesystem Convension
      -[-]version
    Positional:  # Affects Following Tests and Actions
      -daystart  # Measure Time from the Beginning of Today, rather than from 24 hours ago
      -regextype ed|emacs|[gnu-|posix-]awk|grep|posix-[minimal-]basic|[posix-]egrep|posix-extended|sed|findutils-default
      -[no]warn
  Operator:  # Logical Join
    -a  # AND; Default
    -o  # OR

Option:
  Symbolic Link:
    -P  # Never Follow Symbolic Links; Default
    -L  # Follow Symbolic Links; implies -noleaf
    -H  # Follow Explicitly Specified Symbolic Links ONLY
  -D DEBUG_OPTION(,DEBUG_OPTION)*
    Debug Option:
      exec: Debug -exec[dir], -ok[dir]
      opt: Debug Optimisation
      rates: Predicate Succeeded/Failed Rates
      search: Verbose Directory Navigation
      stat: Debug [l]stat system calls
      tree: Debug Expression Tree
      all: Debug All
      help: Explain Debug Options
  -OLEVEL  # Query Optimisation Level: [0, 3]; Default to 1
```
