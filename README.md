# Bootspy
Bootspy Keep track of unattended reboots (crashs)

## Install
### Debian based distributions
- copy `bootspy.sysinit` to `/etc/init.d/bootspy`

<code shell>
cp bootspy.sysinit /etc/init.d/bootspy
</code>

- make the script executable

<code shell>
chmod +x /etc/init.d/bootspy
</code>

- copy `bootspy.default` to `/etc/default/bootspy`

<code shell>
cp bootspy.default /etc/default/bootspy
</code>

- Customize `/etc/default/bootspy`

<pre>
CONTROLFILE=/var/log/bootspy.ctl
LOGFILE=/var/log/bootspy.log
EMAIL="root"
SUBJECT="bootspy report for $HOSTNAME"
</pre>

- Insert script into sysinit process
`insserv bootspy`

### Red Hat / CentOS based distributions
TODO

## Usage
When your system shutdown normally, a file is writed. At next boot, if the file is present, it means that the boot process is occuring after a normal shutdown. If the file isn't present, it means that the system didn't shutdown normally or has crashed.

## Test
Violently powerOff your system !
