### Random Dockerfile notes ###

Great source of info about al2 dockerfiles: `https://github.com/aws/aws-codebuild-docker-images/blob/master/al2/x86_64/standard/3.0/Dockerfile`
Dbus Notes:
```
# First accept that you will have to install systemd :(
yum install procps-ng mlocate dbus dbus-libs util-linux
# Don't need rsync but that would cause systemd to be installed too
# place docker-systemctl-replacement files in /usr/bin and symlink as necessary 
# see the busybox site for how to integrate run-it with systemd - could do this with s6 too.
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
