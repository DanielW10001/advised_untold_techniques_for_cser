# Vim Note

- expression syntax, @
- Ex command special char: %, :t
- Why `subprocess.run(['echo', '"a"'])`: `\a"` rather than `"a"`
    - Env:
        - Python 3 on Windows 10 x64
        - `C:\cygwin64\bin\echo` is the first hit in `Path`
- What is happening after `:term COMMAND`, `:term++shell COMMAND` or `:!COMMAND` in vim?
    - Particularly, what are `vim.exe`, `vimrun.exe`, `ntoskrnl.exe`, `pwsh.exe`, `echo` doing to the meta chars?
    - Env:
        - GVim x64 and Python 3 x64 on Windows 10 x64
        - `set shell=pwsh.exe\ -NoLogo` in `~/.vimrc`
        - `C:\cygwin64\bin\echo` is the first hit in `Path`
    - Phenomenon:
        - Using `subprocess.run` to emulate `CreateProcessW`
        - `:term echo "a"`: `a` rather than `"a"`. Why?
            - `run(["echo", "\"a\""])`: `"a"`
        - `:term "echo a"`: CreateProcess failed. Which explanation is correct?
            - `run(["\"echo", "a\""])`: CreateProcess failed
            - `run(["\"echo a\""])`: CreateProcess failed
            - `run(["echo a"])`: CreateProcess failed
        - `:term++shell "echo a"`: `a` rather than `echo a`. Why?
            - `run(["pwsh.exe", "-NoLogo", "-c", "\"echo a\""])`: `echo a`
            - `run(["pwsh.exe", "-NoLogo", "-c", "\"echo", "a\""])`: `echo a`
        - `:term++shell ""echo a""`: `a` rather than `ParserError`. Why?
            - `run(["pwsh.exe", "-NoLogo", "-c", "\"\"echo a\"\""])`: `ParserError: ...`
        - `:!?`: `pwsh.exe -NoLogo -c "?"...` rather than CreateProcess failed. Why?
            - `run(["?"])`: CreateProcess failed
- To Learn
    - Vim Manual: `:h doc-file-list`
        - Basic
            - `quickref.txt`
            - `tutor.txt`
            - `copying.txt`
            - `iccf.txt`
            - `sponsor.txt`
            - `www.txt`
            - `bugs.txt`
        - User: `:h user-manual`
            - Particularly Worthy: `usr_02.txt`, `usr_41.txt`
        - Reference: `:h reference_toc`
    - [Vim Chinese Manual](http://yianwillis.github.io/vimcdoc/doc/help.html)
    - Subject
        - Argument List

## 1. CLI

- `[g]vim [ARGUMENTS] [FILES]`: Edit `file`s
- `[g]vim [ARGUMENTS] -`: Read text from stdin
- `[g]vim [ARGUMENTS] -t TAG`: Edit file which defined `TAG`
- `[g]vim [ARGUMENTS] -q [ERRORFILE]`: Edit file with first error
- Argument
    - `-c COMMAND`: Execute `COMMAND` before loading vimrc
    - `--cmd COMMAND`: Execute `COMMAND` after loading the first file
    - `-S SESSION`: Source file `SESSION` after loading the first file
    - `-s SCRIPTIN`: Read Normal mode commands from file `SCRIPTIN`
    - `-p[NUM]`: Open `NUM` tabs (pages)
    - `-o[NUM]`: Open `NUM` windows
    - `-O[NUM]`: Open `NUM` windows vertically
    - `-t TAG`: Edit `TAG` definition
    - `-q [ERROR_CONFIG]`: Edit first error

## 2. Start

- Start: `vim <FileDir>`
- Edit File: `:e <FileDir>`
- View File: `:vie[w] <FileDir>`

## 3. Set
- Start up script: `~/.vimrc`

## 4. Move
- Basic
    - Left: `h`
    - Right: `l`
    - Up: `k`
    - Down: `j`
    - Last non-blank character of current line: `g_`
    - `<Number><Space>`: `<Number>` Character Forward
- Line
    - First line: `gg`
    - Last line: `G`
    - Specified line: `<Number>G`, `:<Number>`
    - Line Forward: `<Number><Enter>`
- `g`: Go to
    - `d`: Definition of word under cursor
- Mark
    - Last mark: ``, ''
- Search
    - Next Match: `n`
    - Previous Match: `N`
- Word Search: Search current word
    - Next Match: `*`
    - Previous Match: `#`
- Bracket: Move to paired bracket: `%`
- Window
    - `<C-w><C-w>`: Jump to another window
- Jump
    - `:ju[mps]`: List and Jump
    - Tag: Identifier
        - `|<Tag>|`: Refer
        - `*<Tag>*`: Define
        - Other Way
    - `<C-]>`: Jump to the definition of tag under the cursor
    - `<C-t>`: Jump to [count] older entry in the tag (default 1)
    - `<C-t>`, `<C-o>`: Jump back
    - `<C-i>`: Jump forward
- Window
    - Whole Page
        - Forward: `<C-f>`
        - Backward: `<C-b>`
    - Half Page
        - Down: `<C-d>`
        - Up: `<C-u>`
    - Line
        - Down: `<C-e>`
        - Up: `<C-y>`
    - Move current line to (without moving cursor)
        - Top: `z<Enter>`, `zt`
        - Center: `z.`, `zz`
        - Bottum: `z-`, `zb`
    - Move `<Number>` line to
        - Top: `<Number>z<Enter>`
        - Center: `<Number>z.`
        - Bottum: `<Number>z-`
    - Move cursor to
        - High: `H`
        - Middle: `M`
        - Low: `L`

## 5. Mode
- Exit to Normal Mode or Cancel a partially completed command: `<ESC>`
- Insert: `i`
    - In insert mode
        - Use `<BackSpace>` to delete character

## 6. Exit
- Exit: `:q`
- Exit all: `:qa`
- Force Exit: `:q!`
- Force Exit all: `:qa!`

## 7. Command
- Command Element
    - Number
    - Operator
        - `d`: Delete and store to register
        - `c`: Change and store to register
        - `y`: Yank (copy)
    - Motion
        - `<Number><Motion>`: Repeat `<Motion>` for `<Number>` times
        - `<Number>`: `<Number>` lines forward
        - `w`: Until the start of the next word, EXCLUDING its first character.
        - `W`: Until the start of the next WORD, EXCLUDING its first character.
        - `e`: To the end of the current word, INCLUDING the last character.
        - `E`: To the end of the current WORD, INCLUDING the last character.
        - `b`: To the start of the last word, EXCLUDING its first character.
        - `B`: To the start of the last WORD, EXCLUDING its first character.
        - `$`: To the end of the line, INCLUDING the last character.
        - `0`: To the start of the line
        - `^`: To the first word's first character of current line
        - `H`: To Top line's First Character of screen
        - `M`: To Middle line's First Character of screen
        - `L`: To Last line's First Character of screen
        - `gg`: To First line of file
        - `G`: To Last line of file
        - `<Number>G`: To `<Number>` line of file
        - `<Number><Space>`: To `<Number>` Characters Forward
        - `t<Character>`: To `<Character>` in line, EXCLUDE
        - `T<Character>`: Reversely to `<Character>` in line, EXCLUDE
        - `f<Character>`: To `<Character>` in line, INCLUDE
        - `F<Character>`: Reversely to `<Character>` in line, INCLUDE
        - `;`: Repeat last `ftFT`
        - `,`: Reversely repeat last `ftFT`
        - `/<Pattern>`: To next `<Pattern>`'s match
        - Block
            - Sentence
                - `(`: To the beginning of the last sentence
                - `)`: To the beginning of the next sentence
            - Paragraph
                - `{`: To the beginning of the last paragraph
                - `}`: To the beginning of the next paragraph
        - Paired Item: `%`
            - Bracket: `()`, `[]`, `{}`
            - Comment: `/**/`
        - Go to marks: `<Character>, '<Character>
    - Option
        - `no<Option>` Reverse option
        - `ic` Ignore Case
        - `hls` Highlight Search
        - `is` Incsearch
- `<Motion>`: Move cursor with `<Motion>`
- `<Operator><Motion>`: Operate `<Operator>` on `<Motion>`
- `<Number><Command>`: Repeat `<Command>` for `<Number>` times
- `.`: Repeat last command
- `:sh[ell]`: Open a shell
- `:!<ShellCommand>`: Execute a shell command
    - `%`: Expanded to current file name
    - `%:r`, `%<`: Expanded to current file main name
- `:set <Option>[ <Option>]...`: Set a option for current session
- Wrap Action
    - `<Action>i<Object>`: Do `<Action>` **In** `<Object>`, Exclude Border
    - `<Action>a<Object>`: Do `<Action>` **A** `<Object>`, Include Border
    - `<Action>`
        - `c`: Change and store
        - `d`: Delete and store
        - `y`: Yank
        - `v`: Visual select
    - `<Object>`
        - `<Number><Object>`: Expande `<Number>` times
        - `w`: word
            - Natual Language word
        - `W`: WORD
            - None-Bland char sequence
        - `s`: Sentence
            - Split by `[.!?][)]"']*\s`
        - `p`: Paragraph
            - Split by `\r\r` (Empty Line)
        - `t`: (HTML) Tag
            - E.g.: `<DL> ... </DL>`
        - `<Character>`: Border
- Block Action: `<C-v>`
    - Enter Block Mode: `<C-v>`
    - Use `<Motion>` to select
    - Action
        - `I`: Insert after current character
        - `A`: Append after current character
        - `x`, `d`: Delete selected character
        - `D`: Delete selected lines
- `dd` Delete: Delete the line and store it in a vim register
- `i` Insert
- `I` Insert Before current line's first non-blank character
- `r<Character>` Replace current character with `<Character>`
- `R` Replace
- `a` Append: Append after cursor
- `A` Append: Append after line
- `u` Undo: Undo last command
- `U` Undo: Reset the line
- `o` Open: Open line below and insert
- `O` Open: Open line before and insert
- `<C-r>`: Redo the last undo
- `p`: Put the content of the register
    - (Default) after the cursor
    - (If the register contains a line) to a new line after
- `]p`: Indented `p`
- `P`: Put the content of the register
    - (Default) Before the cursor
    - (If the register contains a line) to a new line before
- `]P`, `[p`, `[P`: Indented `P`
- `r<Character>`: Replace the character with `<Character>`
- `<C-g>`: Show your location in the file and the file status
- `:w[ <FileDir>]` Write: Save changes (To specific file)
    - `:<Number>,<Number> w <FileDir>` Write: Write `<Number>` line to `<Number>` line to `<FileDir>`
- `ZZ`: Save if needed and quit
- `:sav[eas] <FileDir>` Save: Save buffer content to specific file
- `v<Motion><Command>` Visual: Visual select text with `<Motion>` and execute `<Command>` on it
- `J`: Join Current Line and next Line
- `>`, `<`: Increase, Decrease Indent
- `=`: Auto Indent
- `:r <FileDir>` Retrieve: Insert content of `<FileDir>` to the next line
- `:r !<ShellCommand>`: Insert output of `<ShellCommand>` to the next line
- `<C-d>`: Command-Line promopt
- `<Tab>`: Command-Line Complete
- `<Up>`, `<Down>`: Command-Line History
- `:history`: Get Command History
- `gU`: Trans to Upper Case
- `gu`: Trans to Lower Case
- `<C-n>`, `<C-p>`: Insert Mode Completion

## 8. Edit
- Delete Line: `dd`, `:d`
- Delete Character: `x`
- Delete Last Character: `X`

## 9. Copy
- Copy current line: `yy`, `Y`

## 10. Paste
- Paste: `p`

## 11. Change
- Increase number: `<C-a>`
- Decrease number: `<C-x>`
- `:[RANGE]sor[t][!] [b|o|n|o|x][i][r][u] [/PATTERN/]`
    - Range: Default to `%`
    - `!`: Reverse order
    - Order
        - `b`: Binary order
        - `o`: Octal order
        - `n`: Decimal order
        - `x`: Hexadecimal order: `0x`, `0X`
        - `f`: Float order
    - Switch
        - `i`: Ignore case
        - `u`: Unique: Keep first occurrence of lines with the same rank only
        - `r`: Sort within the pattern match
            - Default: Sort after the pattern match
    - Pattern: Default to `^`
        - If given, unmatched lines will be sort out as a dedicated group in the current/reversed order before matched lines
        - `//` for last search pattern

## 12. Indent
- Increase Indent: `>`
- Decrease Indent: `<`
- Auto Indent: `=`

## 13. Window
- 纵向分隔: `:sp <filename>`
- 横向分隔: `:vs <filename>`
- 横向分隔并创建空Buffer: `<C-w>n`
- 切换窗口: `<C-w>`->`[hjkl]` 或 `<C-w><C-w>`
- Size
    - `<C-w>=`: Reset Size
    - `<C-w>+`, `<C-w>-`: Increase, Decreade Horizon Size
    - `<C-w>>`, `<C-w><`, `:vert<C-w>+`, `:vert<C-w>-`: Increase, Decreade Vert Size
    - `<C-w>_`: Max Verticle
    - `<C-w>|`: Max Horizen

## 14. Fold

- Set: `set foldmethod=manual`
- Fold: `zf<Motion>`
    - Fold Brace: `zfa{`
- `zM`: Fold All (More)
- `zR`: Unfold All (Reduce)
- `zm`: Fold One Level (More)
- `zr`: Unfold One Level (Reduce)
- Fold / Defold: `za`
- Save Fold: `:mkvie[w]`
- Load Fold: `:lo`, `:loadview`

## 15. Search
- `<Pattern>`
    - Perl-like with eregex.vim
    - Meta Character
        - `\`: Escape meta character to normal character
        - `.`: Any character EXCEPT `\n`
        - `\_.`: Any Character INCLUDE `\n`
        - `*`: 前一个字符任意次
        - `\{-}`: 厌恶匹配前一个字符任意次
        - `\+`: 前一个字符一到多次
        - `\=`: 前一个字符零或一次
        - `\{<Number>,<Number>}`: 前一个字符 `<Number>` 到 `<Number>` 次
            - 下限省略则为0
            - 上限省略则为正无穷
        - `\{-<Number>, <Number>}`: 厌恶匹配前一个字符 `<Number>` 到 `<Number>` 次
        - `\{<Number>}`: 前一个字符 `<Number>` 次
        - `\(`, `\)`: 用于设置捕获组
        - `\|`: 或
        - `\&`: 且, 只有两个模式都匹配时才匹配, 且匹配后一个模式
        - `[`, `]`: 设置字符组
            - `[abc]`: 普通字符组
            - `[a-z]`, `[0-9]`: 范围字符组
                - 若字符组需要包含`-`字符, 则应将其放在字符组之首或尾
            - `[^a-z]`: 反向字符组
        - `\_[]`: 换行符或字符组
        - `\a`: 大小写字母
        - `\_a`: 换行符或大小写字母
        - `\d`: 数字
        - `\_d`: 换行符或数字
        - `\D`: 非数字
        - `\x`: 大小写十六进制数符
        - `\X`: 非大小写十六进制数符
        - `\s`: 空白符
        - `\_s`: 换行符或空格符
        - `\S`: 非空白符
        - `\l`: 小写字母
        - `\L`: 非小写字母
        - `\u`: 大写字母
        - `\U`: 非大写字母
        - `\h`: `[A-Za-z_]`
        - `\w`: `[0-9A-Za-z_]`
        - `\f`: 合法文件名字符
            - Option `isfname` 决定
        - `\F`: 非数字合法文件名字符
        - `\i`: 合法标识符字符
            - Option `isident` 决定
        - `\I`: 非数字合法标识符字符
        - `\k`: 关键词字符
            - Option `iskeyword` 决定
        - `\K`: 非数字关键词字符
        - `\f`: Filename char
        - `\F`: Non-digits filename char
        - `\p`: 可打印字符
            - Option `isprint` 决定
        - `\P`: 非数字可打印字符
        - `\e`: Esc
        - `\t`: Tab
        - `\r`: CR
        - `\b`: BS
        - `%`
        - `/`: Border
        - `?`
        - `~`
        - `\n`: 换行符
        - `^`: Begining of line
        - `$`: End of line
        - `\<`: Begining of word
        - `\>`: End of word
        - `\@=`: 先行零宽正向匹配前一个字符 (匹配元), `\(<Pattern>\)\@=` 相当于 `(?=<Pattern>)`
        - `\@<=`: 后行零宽正向匹配前一个字符 (匹配元), `\(<Pattern>\)\@<=` 相当于 `(?<=<Pattern>)`
        - `\@!`: 先行零宽反向匹配前一个字符 (匹配元), `\(<Pattern>\)\@!` 相当于 `(?!<Pattern>)`
        - `\@<!`: 后行零宽反向匹配前一个字符 (匹配元), `\(<Pattern>\)\@<!` 相当于 `(?<!<Pattern>)`
        - `\c`: Ingore Case
        - `\C`: With Case
        - `\%^`: Start of the file/string
        - `\%$`: End of the file/string
        - `\%V`: Position inside the Visual area
        - `\%#`: Position of cursor
        - `\%'MARK`: Position of mark
        - `\%<'MARK`: Position before mark
        - `\%>'MARK`: Position after mark
        - `\%NUMl`: Position in `NUM` line
        - `\%<NUMl`: Position before `NUM` line
        - `\%>NUMl`: Position after `NUM` line
        - `\%NUMc`: Position in `NUM` column
        - `\%<NUMc`: Position before `NUM` column
        - `\%>NUMc`: Position after `NUM` column
        - `\%NUMv`: Position in `NUM` virtual column
        - `\%<NUMv`: Position before `NUM` virtual column
        - `\%>NUMv`: Position after `NUM` virtual column
        - `\m`: Magic On
        - `\M`: Magic Off
        - `\v`: Very Magic (Not recomended) (Perl-like) 
        - `\V`: Very NoMagic
- Search: `/<Pattern>`
    - IgnoreCase: `/<Pattern>\c`
    - WithCase: `/<Pattern>\C`
    - Next Match: `n`
    - Previous Match: `N`
- Reverse Search: `?<Pattern>`
- Search Times: `/<Pattern>/<Number>` Search and locate cursor to `<Number>` lines after match
    - `<Number>` 可正可负
- Repeat Search: `/`
- Search Current word
    - Next Match: `*`
    - Previous Match: `#`
    - With partly match: `g*`, `g#`
- Cancel Highlight of Last Search: `:noh`, `:nohls`

## 16. Substitute

- `py3 import re`, `:py3do return re.sub(r'PCRE', 'REPLACEMENT', line)`
- `:[<EffectRange>]S/<Pattern>/<NewContent>/[<Flag>]...`
    - Pettern: Perl-like
- `:[<EffectRange>]s/<Pattern>/<NewContent>/[<Flag>]...`
    - EffectRange
        - Empty (Default): Current line
        - `%`: Current file
        - `<Number>,<Number>`: From `<Number>` line to `<Number>` line
        - `<Number,$`: From `<Number>` line to the last line of file
        - `.,<Number>`: From `.` current line to relatively `<Number>` lines. `<Number>` can be both positive and negative.
    - Pattern: Vim-like
    - Content
        - `\<Number>`: Use Catch Team
    - Flag
        - Empty (Default): Substitute first match in the line from cursor
        - `g` Global: Substitute all matchs in the line from cursor
        - `c` Confirm: Ask for comfirm for every subsitute
        - `i` Search Ignore Case
        - `I` Search Case sensitive
- `<Visual>:s/<Pattern>/<NewContent>/[<Flag>]...`: Substitute in visual selection
- `:perldo s/<Pattern>/<Content>/[<Flag>...]`
    - `<Pattern>`: Perl-like

## 17. Macro
- Recording
    - Start: `q<MacroKey>`
    - Stop: `q`
- Call
    - Call <MacroKey>: `@<MacroKey>`
    - Call last recorded macro: `@@`

## 18. Save
- Save: `:w`
- Save and quit: `:wq`

## 19. Help
- Help: `<F1>` or `:h[elp]`
- Help for `<Subject>`: `:h[elp] [<Prepend>]<Subject>`
    - Prepend
        - Empty: Normal mode command
        - `v_`: Visual mode command
        - `i_`: Insert mode command
        - `:`: Command-line command
        - `c_`: Command-line editing
        - `'`: Option
        - `/`: Regular exression
    - Subject
        - `CTRL-X`

## 20. Fold
- `zf<Motion>`, `<Visual>zf`: 创建折叠区并折叠
- `za`: 开关折叠

## 21. File
- `:e`: Reload current file
- `:n`, `:bn`, `<C-^>`: Next File of Argument List
- `:N`, `:bp`: Previous File of Argument List
- `:vert sf <FileDir>`: Vertically Split window with Finding file
- `:vert sp <FileDir>`: Vertically SPlit window with editing file
- `:e[dit]`: Edit (Reload) file
- `:set f[ile]t[ype]=<FileType>`: Set the file type manually
- `:set fileencoding`: Get current file encoding
- `:set fileencoding=ENCODING`: Transfer file encoding to `ENCODING`
- `:[vert[ical] ][RANGE]ter[minal][OPTIONS] [COMMAND]`: Terminal (Output) Buffer Split
    - `RANGE`: Lines of current buffer will be inputed
    - `OPTIONS`
        - `++shell`: Use shell instead of executable
            - Use quotes to indicate command with spaces: `:term++shell "COMMAND"`
            - Usage with shell and quotes: `:term++shell "git add .; git commit -m \"MESSAGE\"; git push REMOTE BRANCH"`
    - `COMMAND`
        - `%`: Current File Name
            - `%:r`: Current File Name without Extension Name
            - `\%`: Literal `%`

## 22. Buffer

- Buffer is in-memory file
- State
    - Active
        - Loaded and Displayed
    - Hidden
        - Loaded and Not Displayed
    - Inactive
        - Not Loaded and Not Displayed
        - Used to save buffer option, mark, ...
- Number
    - Each buffer has a unip number in Session
    - Will not change within a Vim Session
-  `:buffers`: List buffers
- `:e[dit] FILE`: Create a new buffer with `FILE`
- `:b[uffer] FILE`: Swith buffer viewed by current window

## 23. Window

- Buffer is in-memory file
- Window is buffer viewport
- Tab Page is window collection
- ID
    - Each window has a unique ID in Session
    - Will not change within a Vim Session
- Number
    - Each window has a unique ID in Tab
    - May change when window opend or closed
- Starting Window
    - Default: One Emty Window
    - `vim -o(<Number>)? <File>( <File>)*`
        - Open a horizontal window for each file
        - If `<Number>` given: Open `<Number>` window
    - `vim -O(<Number>)? <File>( <File>)*`
        - Open a vertical window for each file
        - If `<Number>` given: Open `<Number>` window
- Open Window
    - `<C-w>s`, `:<Height>?sp[lit] [file]`
    - `<C-w>v`, `:<Width>?vs[plit] [file]`
        - Split current window in two horizonly or vertically
        - If `file` is not given, result is two window on the same file
        - If `file` is given, it will be open in the new window
        - `:sv[iew] {file}`: Split and view
        - `:sf[ind] {file}`: Split and find
    - `<C-w>n`, `:<Height>?new <File>?`, `:<Width>?vne[w] <File>?`
        - Create a new window with a file
    - `:<Height>?sf[ind] <File>`, `:vert sf[ind] <File>`
        - Find `<File>` and Split
    - `:[vert ]sb[uffer] (<BufferID>|<BufferName>)`
        - Split with buffer
    - `:vert[ical] <Command>`
        - Execute `<Command>` with all split vertically
    - `<C-w><C-^>`, `<C-w>^`: Split and edit next buffer file
- Close Window
    - `:q[uit]`
        - Quit current window
    - `:hid[e]`
        - Quit current window and Hide current buffer if possible
- Moving Cursor
    - `<C-w>(j|k|h|l)`
    - `<C-w><C-w>`: To another window
- Moving Window
    - `<C-w>(J|K|H|L)`
    - `<C-w>T`
        - To a new Tab Page

- `<C-w>=`: Resize all windows to the same size
- `<C-w>o`: Make current window the only window

## 24. Tab (Page)

- Tab Page is window collection

## 25. Mark
- `m<Character>`: Mark current location as `<Character>`
    - `'`: Last Jump
    - `"`: Last Edit
    - `[`: Start of Last Change
    - `]`: End of Last Change
    - `a-z`: Valid within one file
    - `A-Z`
        - File Mark, Book Mark
        - Valid between files
        - Kept in `.viminfo`
    - `0-9`: Set from `.viminfo`
- ``<Character>`, `'<Character>`: Jump to `<Character>` Mark
- ``: Jump to last Mark
- `:marks( <Char>...)?`: List marks
- `delm[arks] <Character>...`: Delete marks

## 26. Register

- For Text Storage
- Registers
    - Default Registers
        - `"`
            - Default register used by `d`, `c`, `s`, `x`, `y` whether or not a specific register was used (except when `_` register was indicated)
            - Contains the same contents as last used register (if indicated and not `_`)
        - `0-9`: Default yank, delete, change registers
            - `0`
                - Default yank register when not indicating specific register
            - `1`
                - Default delete and change register when not indicating specific register
                - Register deletion or change more than one line
            - `2-9`
                - Delete stack
                - With each successive deletion or change, Vim shifts the previous contents of register `1` into register `2`, `2` into `3`, and so forth, losing the previous contens of register 9
        - `-`:
            - Default small delete and change register when not indicating specific register
            - Register deletion and change within one
    - User Registers: `a-z`
        - Only used when specificed
        - Lowercase to replace
        - Uppercase to append
    - Read-only Registers
        - `.`: Last inserted text
        - `%`: Name of current file
        - `:`: Last command
            - E.g.: `@:`: Repeat last command
    - Expression Register: `=`
        - Evaluate expression and store the result as string
        - E.g.: At Insert / Command Mode: `<C-r>=<Expression>`: Insert the value of `<Expression>`
    - Selection and Drop Registers
        - `*`: POSIX system clipboard and Windows system clipboard
        - `+`: POSIX mouse highlight clipboard and Windows system clipboard
    - `_`
        - Black hole register
        - When writing to it, nothing happens
            - Used to delete taxt without addexting the normal registers
        - When reading from it, return `""`
    - `/`
        - Last search pattern register
- Access register: `"<Register><Command>`
    - The `<Command>` will use given register
    - E.g.:
        - `"ayy`: Yank current line into register `a`
        - `"Ayy`: Yank current line into the end of register `a`, which means appends after original contents of register `a`
        - `"ap`: Past register `a`'s content
- `<C-r><Register>`: Paste content of register at insert or command mode
- Set value of register: `:let @<Register> = "<Content>"`
- List Registers: `:reg`, `:di`
- `q<Register>`: Record macro into `<Register>`
- `q`: Stop recording
- `@<Register>`: Execute content of register
- `@@`: Repeat last `@<Register>`
- `:@<Register>`: Execute content of register as an EX command
- `:@`, `:@@`: Repeat last `:@<Register>`

## 27. Key Map

- Leader Key: Default: `\`
- Map
    - `:map <Origin> <Target>`: Map `<Origin>` to `<Target>`
    - `:no[remap] <Origin> <Target>`: Map `<Origin>` to `<Target>` and disallow mapping from `<Target>` to avoid nested mapping.

## 28. Perl

- `:<Range>?perldo <PerlStatement>`
    - Excute `<PerlStatement>` for each line in `<Range>`
    - Default `<Range>`: `%`
- Substitute with PCRE: `:<Range>?perldo? s/<TargetPattern>/<Replacement>/g?`

## 29. Python

在文件中编写处理函数:

```pyton3
def process(line, linenr) -> str:
    # ...
```

在VIM中调用:

```vimscript
:py3f process.py
:.,$py3do return process(line, linenr)
```

- Each instance of vim maintain a python interpreter
    - Have consistent context
- EffectRange
    - Empty (Default), `%`: Current file
    - `<Number>,<Number>`: From `<Number>` line to `<Number>` line
    - `<Number>,$`: From `<Number>` line to the last line of file
    - `.,<Number>`: From `.` current line to relatively `<Number>` lines. `<Number>` can be both positive and negative.

### 29.1. Excute

- Excute Single Line: `:<Range>?py(?:thon)?3 <Statement>`
    - Execute `<Statement>` with "current range" set to `<Range>`
    - Default `<Range>`: `%`
- Excute Multiple Line:
```vim
:<Range>py(?:thon)?3 << <EndMarker>
<Python Script>
<EndMarker>
```
    - Execute `<Python Script>` with "current range" set to `<Range>`
    - Default `<Range>`: `%`
    - Default `<EndMarker>`: `.`
- Excute File: `:<Range>?py3f(?:ile)? <FileDir>`
    - Execute Python script in `<FileDir>` with "current range" set to `<Range>`
    - Default `<Range>`: `%`
    - Excute File with argument:
```vim
:py3 sys.argv = ['<Arg>', ...]
:py3f <FileDir>
```

### 29.2. Edit

- `:<Range>py3do <Body>`
    - Execute code block as follow for each line in `<Range>` with "current range" set to `<Range>`
```python
def _vim_py3do(line, linenr):
    <Body>
_vim_py3do(line, linenr)
```
        - Default `<Range>`: `%`
        - Function Argument
            - `line`: Text of each line without trailing `<EOL>`
            - `linenr`: Current line number
        - Return Value of `_vim_py3do(line, linenr)`
            - Should be `str` or `None`
            - If `str` returned, it becomes the text of current line
    - E.g.:
        - `:py3do return line.replace(r'<Pattern>', r'<Replacement>')`
```vim
:py3 <<
def <Function>():
    # ...
.
:py3do return <Function>()
```

### 29.3. Vim Module

- Python access vim via the `vim` module
- Import before using: `import vim`

#### 29.3.1. Method

##### 29.3.1.1. `command()`

- `None command(<Command>: str)`
    - Execute `:<Command>` in Execution Mode
- `None command(r'normal <Command>': str)`
    - Execute `:<Command>` in Normal Mode

##### 29.3.1.2. `eval()`

- `eval(expression: str) -> (str|list|dict)`
- Evaluate `expression` string with vim internal expression evaluator and return the result
    - List and Dict are recursively expanded when eval
- Return
    - `str`: If eval to string or number
    - `list`: If eval to Vim List
    - `dict`: If eval to Vim Dict

#### 29.3.2. Constant

##### 29.3.2.1. `buffers`

- Sequence of vim buffer objects

##### 29.3.2.2. `windows`

- Sequence of vim windows objects of current tab

##### 29.3.2.3. `tabpages`

- Sequence of vim tab page objects

##### 29.3.2.4. `current`

###### 29.3.2.4.1. Attribute

|   Name    |    Describetion    | Access |   Type    |
| :-------: | :----------------: | :----: | :-------: |
|  `line`   |    Current Line    |   rw   |   `str`   |
| `buffer`  |   Current Buffer   |   rw   | `buffer`  |
| `window`  |   Current Window   |   rw   | `window`  |
| `tabpage` |  Current Tab Page  |   rw   | `tabpage` |
|  `range`  | Current Line Range |   r    |  `range`  |

- `range`: Buffer Subset set by `<Range>`
- Assigning to `vim.current.(buffer|window|tabpage)`
    - Expect valid `(buffer|window|tabpage)` object
    - Trigers switching in UI

#### 29.3.3. Class

##### 29.3.3.1. `error`

- Exception Class

## 30. Comment

- `:" <Comment>`

## 31. Expression

- Refered as `expr` in vim docs

### 31.1. Syntax

```abnf
expr1 = expr2 / expr2 "?" expr1 ":" expr1; if-then-else
|expr1|	expr2
	expr2 ? expr1 : expr1	if-then-else

|expr2|	expr3
	expr3 || expr3 ...	logical OR

|expr3|	expr4
	expr4 && expr4 ...	logical AND

|expr4|	expr5
	expr5 == expr5		equal
	expr5 != expr5		not equal
	expr5 >	 expr5		greater than
	expr5 >= expr5		greater than or equal
	expr5 <	 expr5		smaller than
	expr5 <= expr5		smaller than or equal
	expr5 =~ expr5		regexp matches
	expr5 !~ expr5		regexp doesn't match

	expr5 ==? expr5		equal, ignoring case
	expr5 ==# expr5		equal, match case
	etc.			As above, append ? for ignoring case, # for
				matching case

	expr5 is expr5		same |List|, |Dictionary| or |Blob| instance
	expr5 isnot expr5	different |List|, |Dictionary| or |Blob|
				instance

|expr5|	expr6
	expr6 +	 expr6 ...	number addition, list or blob concatenation
	expr6 -	 expr6 ...	number subtraction
	expr6 .	 expr6 ...	string concatenation
	expr6 .. expr6 ...	string concatenation

|expr6|	expr7
	expr7 *	 expr7 ...	number multiplication
	expr7 /	 expr7 ...	number division
	expr7 %	 expr7 ...	number modulo

|expr7|	expr8
	! expr7			logical NOT
	- expr7			unary minus
	+ expr7			unary plus

|expr8|	expr9
	expr8[expr1]		byte of a String or item of a |List|
	expr8[expr1 : expr1]	substring of a String or sublist of a |List|
	expr8.name		entry in a |Dictionary|
	expr8(expr1, ...)	function call with |Funcref| variable
	expr8->name(expr1, ...)	|method| call

|expr9|	number			number constant
	"string"		string constant, backslash is special
	'string'		string constant, ' is doubled
	[expr1, ...]		|List|
	{expr1: expr1, ...}	|Dictionary|
	#{key: expr1, ...}	|Dictionary|
	&option			option value
	(expr1)			nested expression
	variable		internal variable
	va{ria}ble		internal variable with curly braces
	$VAR			environment variable
	@r			contents of register 'r'
	function(expr1, ...)	function call
	func{ti}on(expr1, ...)	function call with curly braces
	{args -> expr1}		lambda expression
```

- Value of option: `&<Option>`
- Value of register: `@<Register>`

## 32. Key Notation

- Help: `:h key-notation`
- Meta == Alt

## 33. Session

- `:mks[ession][!] [file]`: Make session file or `Session.vim`
- `vim -S <SessionFile>`, `:so[urce] <SessionFile>`: Restore Session
- Can be accessed with Startify

## 34. Diff

- `vim -d <File>{ <File>}`
- `:[vert ]diffs[plit]{ <File>}`: Diff between current and new file

## 35. Repeat

- `:[RANGE]g[lobal][!]/PATTERN/[COMMAND]`: Execute Ex command on lines in range where pattern (not) matches
    - Range: Default to `%`
    - `!`: Execute on unmatched lines
        - Default to executing on matched lines
    - Pattern
        - `//` for last search pattern
    - Command: Default to `:p`
- `:[RANGE]v[global]/PATTERN/[COMMAND]`: Equivalence of `:g!`

## 36. various

- `:[RANGE]p[rint] [FLAGS]`: Print lines in range
    - Range: Default to current line

## 37. Config

- `~/.vimrc`

```vimrc
set foldmethod=syntax
```

## 38. Reference

- vimtutor
- VIM Manual: Pattern
- 简明 VIM 练级攻略
