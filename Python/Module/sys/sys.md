# sys

```python3
# Audit
sys.addaudithook(hook)
sys.audit(event, *args)  # Raise an audit event

# Site Info
sys.modules  # Dictionary from module names to module object; Can be used to force reload module
sys.path  # List of module search path
sys.executable  # Path of Python Interpreter
sys.prefix  # Platform independent installation dir
sys.exec_prefix  # Platform dependent installation dir
sys.base_prefix  # Original platform independent installation dir
sys.base_exec_prefix  # Original platform dependent installation dir
sys.implementation  # Interpreter implementation info
sys.platform  # Platform name string
sys.byteorder  # 'big' or 'little'
sys.builtin_module_names  # Tuple of built-in module names
sys.flags  # Command line option values
sys.float_info  # Float type info
sys.int_info  # Int type info
sys.hash_info  # Hash info
sys.thread_info  # Thread implementation info
sys.api_version  # C API version of interpreter
sys.version  # Python interpreter version
sys.version_info  # Python interpreter version; Named tuple: (major, minor, micro, releaselevel, serial)
sys.winver  # Version number used to form Windows registry keys
sys.getdefaultencoding()  # Get default encoding
sys.getfilesystemencoding()  # Get file system encoding
sys.getfilesystemencodeerrors()  # Get file system encode error mode
sys.getrecursionlimit()  # Get recursion limit
sys.setrecursionlimit(limit)  # Set recursion limit
sys.tracebacklimit  # Print traceback limit
sys.getwindowsversion()  # Get Windows version

# IO
sys.stdin
sys.stdout
sys.stderr
sys.stdin.buffer  # Binary stdin
sys.stdout.buffer  # Binary stdout
sys.stderr.buffer  # Binary stderr
sys.__stdin__  # Original stdin
sys.__stdout__  # Original stdout
sys.__stderr__  # Original stderr

# Program
sys.argv  # List of command line arguments
sys.exit([exit_code|"Exit message"])  # Exit program
```
