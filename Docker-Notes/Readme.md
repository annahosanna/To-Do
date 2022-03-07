### Random Dockerfile notes ###

Great source of info about al2 dockerfiles: `https://github.com/aws/aws-codebuild-docker-images/blob/master/al2/x86_64/standard/3.0/Dockerfile`
Dbus Notes:
```
* First accept that you will have to install systemd :(
 1. yum install procps-ng mlocate dbus dbus-libs util-linux
 2. Many tools like rsync and OpenSSH (distro), have for no good reason, been forced to use Systemd as a dependancy by the distro vendor.
* Systemd was not made to use containers and rely on the abilty to see cgroups, in the same way Docker does. This allows Systemd Units for which a parent process has exitted but the child remains to be correctly managed.
 1. While systemd is useless - systemctl, and journalctl are still necessary, so they should be replaced with:
  * docker-systemctl-replacement, and docker-journalctl-replacement files in /usr/bin and symlink as necessary
  * Further process management can also be done with s6, s6-overlay, and run-it
  * [Integrating Runit with Systemd Example](https://busybox.net/kill_it_with_fire.txt)

# basically if you are running without cgroups then do not daemonize services 
# so that the process supervisor can do its job 
# When building the dockerfile you might need to fake dbus
# dbus-uuidgen > /var/lib/dbus/machine-id 
# dbus-daemon --config-file=/usr/share/dbus-1/system.conf --print-address
# Then do whatever, and when done rm /run/dbus/messagebus.pid
# Adjust /etc/systemd/journald.conf since it cannot write to kmsg and syslog lines should not exeed 1K 
# THis is not a graphical system: systemctl mask graphical.targget runlevel5.target
# systemctl set-default multi-user.target
# If installing ssm, otel, cloudwatch don't forget to put at least the default configurations in place
