# Bootspy

Bootspy Keep track of unattended reboots (crashs).

When the system shuts normally, a file is writen. During the next boot, if the
file is present, it means that the boot process is occuring after a normal
shutdown. If the file isn't there, it means the system didn't shutdown normally
or has crashed. In this case, an alert can be triggered.

# Dependencies

Local MTA (`postfix`|`exim`|`nullmailer`) and mail command (`bsd-mailx`) to be
notified by mail.

Ntfy.sh data to be notified by Ntfy.

Otherwise only logs to file.

# Install

## On Systemd based distributions

Copy `systemd/bootspy` to `/usr/local/sbin`
```sh
cp systemd/bootspy /usr/local/sbin/
```
Make the script executable
```sh
chmod +x /usr/local/sbin/bootspy
```
Copy `systemd/bootspy.service` to `/etc/systemd/system`
```sh
cp systemd/bootspy.service /etc/systemd/system
```
Copy `bootspy.default` to `/etc/default/bootspy`
```sh
cp bootspy.default /etc/default/bootspy
```

## On Sysinit based distributions

NOTE: sysinit part is unmaintained since early 2025.

Copy `sysinit/bootspy.sysinit` to `/etc/init.d/bootspy`
```sh
cp sysinit/bootspy.sysinit /etc/init.d/bootspy
```
Make the script executable
```sh
chmod +x /etc/init.d/bootspy
```
Copy `bootspy.default` to `/etc/default/bootspy`
```sh
cp bootspy.default /etc/default/bootspy
```

# Configure

Customize `/etc/default/bootspy`
```sh
controlfile="/var/lib/bootspy/control"
logfile="/var/log/bootspy.log"
# Notifications type(s). Choose between email, ntfy or both.
# notifications="email,ntfy"
notifications="email"
# email notifications needs only the email address or alias.
email="root"

# Ntfy needs more informations
# ntfy_token="tk_#############################"
# ntfy_url="https://ntfy.example.org/bootspy"
# ntfy_normal_priority="default"
# ntfy_normal_tags="arrows_counterclockwise"
# ntfy_crash_priority="high"
# ntfy_crash_tags="arrows_counterclockwise,boom"
```

# Enable at boot

## Systemd based distributions

Reload systemd daemon
```sh
systemctl daemon-reload
```
Enable systemd unit
```sh
systemctl enable bootspy.service
```

## Sysinit based distributions

Insert script into sysinit process
```sh
insserv bootspy
```

# Tests

Reboot the system to test normal boot detection.

Unplug the power or reset the system to test crash detection !

Or use these commands as root to simulate a crash :
```sh
systemctl stop bootspy.service
rm /var/lib/bootspy/control
systemctl start bootspy.service
```

Events are logged in `/var/log/bootspy.log`.

```sh
[2025-01-27 23:49:59] shutdown
[2025-01-27 23:50:13] boot
[2025-02-03 20:52:09] boot after crash ?
[2025-02-17 19:03:35] shutdown
[2025-02-17 19:03:51] boot
```

# Uninstall

## Systemd based distributions

Remove executable from systemd startup sequence
```sh
systemctl disable bootspy.service
```
Remove `/usr/local/sbin/bootspy`
```sh
rm /usr/local/sbin/bootspy
```
Remove `/etc/systemd/system/bootspy.service`
```sh
rm /etc/systemd/system/bootspy.service
```
Remove `/etc/default/bootspy`
```sh
rm /etc/default/bootspy
```
Reload systemd daemon
```sh
systemctl daemon-reload
```

## Sysinit based distributions

Remove initscript from startup sequence
```sh
insserv -r bootspy
```
Remove `/etc/init.d/bootspy`
```sh
rm /etc/init.d/bootspy
```
Remove `/etc/default/bootspy`
```sh
rm /etc/default/bootspy
```
