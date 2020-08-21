# cmd

```python3
class CustomCMD(cmd.Cmd):
    prompt = 'prompt symbol'
    intro = 'intro'
    use_rawinput = False  # System readline

    def do_command(self, argstr):  # Handle `command`
        '''Introduction
        '''
        # Return `True` if interpretion should end, `False` (usually `None`) otherwise: return True
    def help_command(self):  # Print help message of `command`
    def complete_command(self, text, line, begidx, endidx):  # Complete `command` args; text: prefix to match; returns list of candidates
        return [candidate for candidate in candidates if candidate.startswith(text)]

    def do_EOF(self, argstr):
        '''Exit
        '''
        return True

    def do_shell(self, argstr):  # Handle `!`

    def emptyline(self):  # Customized handler for empty command; default behavior is repeating last command
    def default(self, line):  # Customized handler for unrecognized command; default behavior is print error message
    def completedefault(self, text, line, begidx, endidx):  # Customizedly Complete args of command without matched complete_* method; defaultly returns empty list

    def precmd(self, line):  # Precmd process; returned string will be interpreted as command
    def postcmd(self, stop, line):  # Postcmd process; `stop` is the return value of command handler; returned value of this method will be interpretion end flag
    def preloop():  # Preloop process
    def postloop():  # Postloop process

cmder = CustomeCMD()
cmder.cmdloop([intro='intro'])
cmder.onecmd(cmdstr)  # Interpret one command; Return what the handler returned
cmder.lastcmd  # Last non-empty command
cmder.cmdqueue  # List of unprocessed input commands
```
