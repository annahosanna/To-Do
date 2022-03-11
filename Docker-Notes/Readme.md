### Random Dockerfile notes ###

* First some programs think they need Systemd, and then you will have to install it :(
 1. yum install procps-ng mlocate dbus dbus-libs util-linux
 2. Many tools like rsync and OpenSSH (distro), have for no good reason, been forced to use Systemd as a dependancy by the distro vendor.
* Systemd was not made to use containers and rely on the abilty to see cgroups, in the same way Docker does. This allows Systemd Units for which a parent process has exitted but the child remains to be correctly managed.
 1. While systemd is useless - systemctl, and journalctl are still necessary, so they should be replaced with:
 2. docker-systemctl-replacement, and docker-journalctl-replacement files in /usr/bin and symlink as necessary
 3. Further process management can also be done with s6, s6-overlay, and Runit
 4. [Integrating Runit with Systemd Example](https://busybox.net/kill_it_with_fire.txt)
* Replacing Systemd -
 1. Systemd monitors processes and cleans up zombie processes. 
* A new PID 1
 1. Having a PID 1 is probably a good idea because of the way environment variable injection happens in containers. Zombie cleanup can be a child process of PID 1.
 2. Tini is an easy choice.
* A new process supervisor 
 1. This is not a graphical system: 
 2. systemctl mask graphical.targget runlevel5.target
 3. systemctl set-default multi-user.target
* Journald is still needed: systemctl start systemd-journald.service
* Make sure the syslog socket is point to the correct place: rm -f /dev/log && ln -s /run/systemd/journal/dev-log /dev/log
* mkdir -p /var/log/journal && chown root:systemd-journal /var/log/journal && chmod 755 /var/log/journal && chmod g+s /var/log/journal
* Adjust /etc/systemd/journald.conf to ensure it does not write to kmsg. The Syslog RFC also states that syslog lines should not exeed 1K. 
* What about DBus
 1. Some programs rely on DBus for signaling; however, this will have to occur without Systemd. The steps are as follows
 2. One time setup of dbus uuid: dbus-uuidgen > /var/lib/dbus/machine-id 
 3. Start DBus: dbus-daemon --config-file=/usr/share/dbus-1/system.conf --print-address
 4. ENV DBUS_SESSION_BUS_ADDRESS="unix:path=/run/dbus/system_bus_socket"
 5. If this was done as a part of building the container then rm -f /run/dbus/messagebus.pid && rm -rf /run/dbus
* Maybe you don't need Systemd
 1. You can build OpenSSH yourself or use a different sshd (You don't need tcl/tk, or GetText normally)
 2. Busybox provides a huge number of programs, including Runit. Some other version of those programs require systemd.
 3. You do not even need to build it yourself. You can use COPY --from ... to get the correct files from the Busybox docker distribution.  
* What about CI/CD for Docker In Docker (DID) and Rootless containers (Which might be needed in Fargate)
 1. Use proot to create a false root - remapping the same files and directories which Docker does: /etc/mtab, /etc/resolv.conf and /etc/hosts (and a few directories too)
 2. Add SYS_PTRACE capability (the only capability which you can add in Fargate)
 3. Use Kaniko to perform the build (to a tar)
 4. Add any non standard library paths (such as Java may create) to ld.so.conf.d
 5. Use GCR crane to upload to Docker repositories.
* Great source of info about al2 dockerfiles: `https://github.com/aws/aws-codebuild-docker-images/blob/master/al2/x86_64/standard/3.0/Dockerfile`
