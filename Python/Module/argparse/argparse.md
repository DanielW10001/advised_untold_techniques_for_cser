# argparse

```python3
import argparse

ARGS = argparse.Namespace()

def function() -> None:
    ARGS.name  # Access
    if ARGS.flag:
        pass

def test() -> None:
    parser = argparse.ArgumentParser(description="DESCRIPTION")
    parser.add_argument('name', help='HELP')
    parser.add_argument('-f', '--flag', action='store_true',
                        help='HELP')
    parser.parse_args(namespace=ARGS)

if __name__ == '__main__':
    test()
```

```python3
parser = argparse.ArgumentParser(
    [description="Program Description"][,
    epilog="Epilogue"][,  # Text after arg description
    parents=[parent_argparser(, parent_argparser)*]][,
    fromfile_prefix_chars='@'][,
    argument_default=value][,
    conflict_handler='resolve'][,
    add_help=False])
parser.set_defaults(attribute=value(, attribute=value)*)  # Set default attributes for parsed object
parser.get_default('attribute')  # Get default value of `attribute`

parser.add_argument(
    ('name'|['-f'][, '--flag'])[,  # Name of positional argument, or flags of optional argument
    action='store_const|store_true|store_false|append|extend|append_const|count|help|version'|ActionClass][,  # Default to `'store'`
    nargs=number|'?'|'*'|'+'|argparse.REMAINDER][,  # Consume number
    const=const][,  # 'store_const', 'append_const', '--flag'
    default=value|argparse.SUPPRESS][,  # Absent value; Default to `None`
    type=type|argparse.FileType([mode=mode][, encoding='utf-8'])][,  # Value type
    choices=[choice(, choice)*]][,  # Container of legal values
    required=True][,  # For optional argument only; Positional argument is always required
    help='Description'|argparse.SUPPRESS][,  # Support Format Specifier
    metavar='metavar'|('metavar'(, 'metavar')*)][,  # Message name; Default to dest (posi arg) or DEST (op arg)
    dest='dest'])  # Stored name; Default to `name`, `flag` or `f`

args = parser.parse_args(
    [args=['arg'(, 'arg')*]][,  # Default to sys.argv
    namespace=object])
args, remaining_argstrs = parser.parse_known_args(  # Parse known args only
    [args=['arg'(, 'arg')*]][,  # Default to sys.argv
    namespace=object])
args = parser.parse_intermixed_args(
    [args=['arg'(, 'arg')*]][,  # Default to sys.argv
    namespace=object])
args, remaining_argstrs = parser.parse_known_intermixed_args(  # Parse known args only
    [args=['arg'(, 'arg')*]][,  # Default to sys.argv
    namespace=object])

args.argument  # Access argument

class CustomArgumentParser(argparse.ArgumentParser):
    def convert_arg_line_to_args(self, arg_line):
        # Customize fromfile arg parse
        return arg_line.split()
    def exit(self, status=0, message=None):
        # Customize exit behavior

class CustomAction(argparse.Action):
    def __init__(self, option_strings, dest, nargs=None, const=None, default=None, type=None, choices=None, required=False, help=None, metavar=None):
        if condition(...):
            raise Error(msg)
        super().__init__(...)
    def __call__(self, parser, namespace, values, option_string=None):
        # ...
        # Default: setattr(namespace, self.dest, values)

# Sub-command
subparsers = parser.add_subparsers([
    title='title'][,
    description='description'][,
    help='help message'][,
    action='store_const|store_true|store_false|append|extend|append_const|count|help|version'|ActionClass][,  # Action taken when subcommand name encountered
    metavar='metavar'][,  # Message name
    dest='dest'][,  # Stored name of subcommand name string; Default to `None`
    required=True])  # If this subcommand name is required
subcommand_parser = subparsers.add_parser(
    'subcommand'[,
    aliases=['aliase'(, 'aliase')*]][,
    help='Help message'][,
    description="Program Description"][,
    epilog="Eplogue"][,
    parents=[parent_argparser(, parent_argparser)*]][,
    fromfile_prefix_chars='@'][,
    argument_default=value][,
    conflict_handler='resolve'][,
    add_help=False])
subcommand_parser.add_argument(...)
subcommand_parser.set_defaults(func=subcommand_function)  # Subcommand specific handler; Invoke with args.func(...)
# Attention: Parsed object will only contain attributes for main parser and **specified** subparser, not other subparsers

# Argument group
group = parser.add_argument_group(
    [title='title'][,
    description='discription'])
group = parser.add_mutually_exclusive_group([required=True])
group.add_argument(...)

# Print help
parser.print_usage([file=outfile])
usage_str = parser.format_usage()
parser.print_help([file=outfile])
help_str = parser.format_help()

# Exiting parse
parser.exit([status=code][, message=message])  # Exit
parser.error(message=message)  # Exit with `message` and usage info
```

Specifying value:

```bash
python program.py -f F -fF -xyz --flag FLAG --flag=FLAG -- -1
```

Format specifier used in description/help message:

```plain
# Format specifier
%%  # Literal %
%(prog)s  # Program Name
%(KEYWORD)s  # Keyword argument to add_argument
```

Default fromfile argument format:  (Can be customized by overriding `argparse.ArgumentParser.conver_arg_line_to_args`)

```plain
(--option
value)*
```

- Program should be useful at default when run without any options
- Positional argument: The meaning of value is determined by its position: `value`
- Use positional argument to ask program for beyond default functionality
- Optional argument: The meaning of value is determined by its switch: `-o value`, `--option value`
- Use optional argument to ask program to change its behavior from default
- Use `-h`, `--help` to provide help message
