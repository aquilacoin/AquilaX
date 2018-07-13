Sample init scripts and service configuration for Aquilad
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/Aquilad.service:    systemd service unit configuration
    contrib/init/Aquilad.openrc:     OpenRC compatible SysV style init script
    contrib/init/Aquilad.openrcconf: OpenRC conf.d file
    contrib/init/Aquilad.conf:       Upstart service configuration file
    contrib/init/Aquilad.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "Aquila" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, Aquilad requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, Aquilad will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that Aquilad and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If Aquilad is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/Aquila/Aquila.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/Aquila.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/Aquilad
Configuration file:  /etc/Aquila/Aquila.conf
Data directory:      /var/lib/Aquilad
PID file:            /var/run/Aquilad/Aquilad.pid (OpenRC and Upstart)
                     /var/lib/Aquilad/Aquilad.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the Aquila user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
Aquila user and group.  Access to Aquila-cli and other Aquilad rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start Aquilad" and to enable for system startup run
"systemctl enable Aquilad"

4b) OpenRC

Rename Aquilad.openrc to Aquilad and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/Aquilad start" and configure it to run on startup with
"rc-update add Aquilad"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop Aquilad.conf in /etc/init.  Test by running "service Aquilad start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy Aquilad.init to /etc/init.d/Aquilad. Test by running "service Aquilad start".

Using this script, you can adjust the path and flags to the Aquilad program by
setting the AquilaD and FLAGS environment variables in the file
/etc/sysconfig/Aquilad. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
