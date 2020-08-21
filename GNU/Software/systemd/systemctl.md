# systemctl: System Control

```bash
# Unit (Service)
systemctl status|is-(active|enabled|failed) [UNIT]
systemctl start|stop|restart|reload|reload-or-restart UNIT
systemctl enable|disable|mask|unmask UNIT
systemctl [list-unit(s|-files)] [-a|--all] [--type=service|target|timer|...] [--state=failed|loaded|[in]active|(en|dis)abled[-runtime]|static|masked|generated|indirect] [PATTERN]  # List Units [Files]; Default to active units; --all for loaded units; -files for available units
systemctl cat UNIT  # Print Parsed Unit File Content
systemctl show UNIT [(-p |--property=)PROPERTY]  # Show Parsed Unit Parameters
systemctl list-dependencies [-a|--all] [--reverse] [--before] [--after] UNIT...
systemctl edit [--full] UNIT...  # Overriding Unit Parameters; --full for Overriding with Unit File `/etc/systemd/system/UNIT` instead of Snippet `/etc/systemd/system/UNIT.d/override.conf`
rm -[r]f /etc/systemd/system/UNIT[.d/override.conf]  # Remove Overriding
systemctl revert UNIT...  # Reset Unit File

# System State
[systemctl] poweroff|reboot|rescue|halt|daemon-reload
systemctl get-default [UNIT]  # Get Default Booting Unit (target)
systemctl set-default UNIT  # Set Default Booting Unit (target)
systemctl isolate UNIT  # Start `UNIT` and Stop All Others
```
