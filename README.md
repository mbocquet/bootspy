# Bootspy

Bootspy Keep track of unattended reboots (crashs).

When the system shuts normally, a file is writen. During the next boot, if the
file is present, it means that the boot process is occuring after a normal
shutdown. If the file isn't there, it means the system didn't shutdown normally
or has crashed. In this case, an alert can be triggered.

## Dependencies

Local MTA (postfix|exim|nullmailer) and mail command to be notified by mail.
Ntfy.sh data to be notified by Ntfy. Otherwise only logs to file.

## Install

### Debian based distributions with systemd

* Copy `systemd/bootspy` to `/usr/local/sbin`  
`cp systemd/bootspy /usr/local/sbin/`
* Make the script executable  
`chmod +x /usr/local/sbin/bootspy`
* Copy `systemd/bootspy.service` to `/etc/systemd/system`  
`cp systemd/bootspy.service /etc/systemd/system`
* Copy `bootspy.default` to `/etc/default/bootspy`  
`cp bootspy.default /etc/default/bootspy`
* Customize `/etc/default/bootspy`  

CONTROLFILE=/var/lib/bootspy/control
LOGFILE=/var/log/bootspy.log
EMAIL="root"

* Reload systemd daemon  
`systemctl daemon-reload`
* Enable systemd unit  
`systemctl enable bootspy.service`

### Debian based distributions with sysinit

NOTE: sysinit part is unmaintained since early 2025.

* Copy `sysinit/bootspy.sysinit` to `/etc/init.d/bootspy`  
`cp sysinit/bootspy.sysinit /etc/init.d/bootspy`
* Make the script executable  
`chmod +x /etc/init.d/bootspy`
* Copy `bootspy.default` to `/etc/default/bootspy`  
`cp bootspy.default /etc/default/bootspy`
* Customize `/etc/default/bootspy`  

    controlfile="/var/lib/bootspy/control"
    logfile="/var/log/bootspy.log"
    # Notifications type(s). Choose between email, ntfy or both.
    # notifications="email,ntfy"
    notifications="email"
    # email notifications needs only the email address or alias.
    email="root"

    # Ntfy needs more informations
    # ntfy_token="tk_#############################"
    # ntfy_url="https://ntfy.sh/mytopic"
    # ntfy_normal_priority="default"
    # ntfy_normal_tags="arrows_counterclockwise"
    # ntfy_crash_priority="high"
    # ntfy_crash_tags="arrows_counterclockwise,boom"

* Insert script into sysinit process  
`insserv bootspy`

## Tests

* Reboot the system to test normal boot detection.
* Unplug the power to test crash detection !
  Or use these commands as root to simulate a crash :
```sh
systemctl stop bootspy.service; rm /var/lib/bootspy/control; systemctl start bootspy.service
```

## Uninstall

### Debian based distributions with systemd

* Remove executable from systemd startup sequence
`systemctl disable bootspy.service`
* Remove `/usr/local/sbin/bootspy`  
`rm /usr/local/sbin/bootspy`
* Remove `systemd/bootspy.service` to `/etc/systemd/system`  
`rm /etc/systemd/system/bootspy.service`
* Remove `/etc/default/bootspy`  
`rm /etc/default/bootspy`
* Reload systemd daemon  
`systemctl daemon-reload`

### Debian based distributions with sysinit

NOTE: sysinit part is unmaintained since early 2025.

* Remove initscript from startup sequence
`insserv -r bootspy`
* Remove `/etc/init.d/bootspy`  
`rm /etc/init.d/bootspy`
* Remove `/etc/default/bootspy`  
`rm /etc/default/bootspy`
