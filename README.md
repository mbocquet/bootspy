# Bootspy

Bootspy Keep track of unattended reboots (crashs).

When your system shutdown normally, a file is writed. During next boot, if the
file is present, it means that the boot process is occuring after a normal
shutdown. If the file isn't present, it means that the system didn't shutdown
normally or has crashed. In this case, a mail is sent to a specific address at
boot time.

## Install

### Debian based distributions with sysinit

- Copy `sysinit/bootspy.sysinit` to `/etc/init.d/bootspy`  
`cp sysinit/bootspy.sysinit /etc/init.d/bootspy`
- Make the script executable  
`chmod +x /etc/init.d/bootspy`
- Copy `bootspy.default` to `/etc/default/bootspy`  
`cp bootspy.default /etc/default/bootspy`
- Customize `/etc/default/bootspy`  
<pre>
CONTROLFILE=/var/lib/bootspy/control
LOGFILE=/var/log/bootspy.log
EMAIL="root"
</pre>
- Insert script into sysinit process  
`insserv bootspy`

### Debian based distributions with systemd

- Copy `systemd/bootspy` to `/usr/local/sbin`  
`cp systemd/bootspy /usr/local/sbin/`
- Make the script executable  
`chmod +x /usr/local/sbin/bootspy`
- Copy `systemd/bootspy.service` to `/etc/systemd/system`  
`cp systemd/bootspy.service /etc/systemd/system`
- Copy `bootspy.default` to `/etc/default/bootspy`  
`cp bootspy.default /etc/default/bootspy`
- Customize `/etc/default/bootspy`  
<pre>
CONTROLFILE=/var/lib/bootspy/control
LOGFILE=/var/log/bootspy.log
EMAIL="root"
</pre>
- Reload systemd daemon  
`systemctl daemon-reload`
- Enable systemd unit  
`systemctl enable bootspy.service`

## Tests

- Reboot the system to test normal boot detection.
- Unplug the power to test crash detection !

## Uninstall

### Debian based distributions with sysinit

- Remove initscript from startup sequence
`insserv -r bootspy`
- Remove `/etc/init.d/bootspy`  
`rm /etc/init.d/bootspy`
- Remove `/etc/default/bootspy`  
`rm /etc/default/bootspy`

### Debian based distributions with systemd

- Remove executable from systemd startup sequence
`systemctl disable bootspy.service`
- Remove `/usr/local/sbin/bootspy`  
`rm /usr/local/sbin/bootspy`
- Remove `systemd/bootspy.service` to `/etc/systemd/system`  
`rm /etc/systemd/system/bootspy.service`
- Remove `/etc/default/bootspy`  
`rm /etc/default/bootspy`
- Reload systemd daemon  
`systemctl daemon-reload`
