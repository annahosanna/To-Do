### Random Dockerfile notes

#### The install requires Systemd :(

- Some programs think they need Systemd, and then you will have to install it :(

1.  yum install procps-ng mlocate dbus dbus-libs util-linux
2.  Many tools like rsync and OpenSSH (distro), have for no good reason, been forced to use Systemd as a dependancy by the distro vendor.

- Systemd was not made to use containers and relies on the abilty to see cgroups, in the same way Docker does. This allows Systemd Units to correctly manage process group resources for which a parent has exited.

1.  While systemd is useless - systemctl, and journalctl are still necessary (and maybe udev too), so they should be replaced with:
2.  docker-systemctl-replacement, and docker-journalctl-replacement files in /usr/bin and symlinked as necessary
3.  Further process management can also be done with s6, s6-overlay, Runit, and GNU Shepherd
4.  [Integrating Runit with Systemd Example](https://busybox.net/kill_it_with_fire.txt)

#### Replacing Systemd

1.  Systemd monitors processes and cleans up zombie processes.

- A new PID 1

1.  Having a PID 1 is probably a good idea because of the way environment variable injection happens in containers.
2.  Zombie cleanup can, but does not need to a be, a child process of PID 1.
3.  Tini is an easy choice.

- A new process supervisor

1.  This is not a graphical system:
2.  `systemctl mask graphical.targget runlevel5.target`
3.  `systemctl set-default multi-user.target`

- Journald is still needed: systemctl start systemd-journald.service
- Make sure the syslog socket is pointing to the correct place: `rm -f /dev/log && ln -s /run/systemd/journal/dev-log /dev/log`
- `mkdir -p /var/log/journal && chown root:systemd-journal /var/log/journal && chmod 755 /var/log/journal && chmod g+s /var/log/journal`
- Adjust /etc/systemd/journald.conf to ensure it does not write to kmsg. The Syslog RFC also states that syslog lines should not exeed 1K.

#### DBus setup

1.  Some programs rely on DBus for signaling; however, this will have to occur without Systemd. The steps are as follows
2.  One time setup of dbus uuid: `dbus-uuidgen > /var/lib/dbus/machine-id`
3.  Start DBus: `dbus-daemon --config-file=/usr/share/dbus-1/system.conf --print-address`
4.  `ENV DBUS_SESSION_BUS_ADDRESS="unix:path=/run/dbus/system_bus_socket"`
5.  If this was done as a part of building the container then `rm -f /run/dbus/messagebus.pid && rm -rf /run/dbus`
6.  Create a script in /etc/profile.d with something like: `export $(strings /proc/1/environ | grep DBUS_SESSION_BUS_ADDRESS)`

#### Maybe you don't need Systemd

1.  Can you build the generic version yourself, so that Systemd is not tied in?
2.  OpenSSH and git are good examples (You don't need tcl/tk, or GetText normally) - and just follow the instructions for installing dependancies like curl-devel
3.  As mentioned above, procps requires Systemd - why not just use the ps from busybox which does not need systemd
4.  Busybox provides a huge number of programs, including Runit.
5.  If your creating a container, then you do not even need to build it yourself. You can use COPY --from ... to get the correct files from the Busybox docker distribution.

#### Docker in Docker (DID) and Rootless containers for CI/CD and Fargate

1.  Use proot to create a false root (it uses the only available Fargate capability `SYS_PTRACE`) - Then remap the same files and directories which Docker does: `/etc/mtab`, `/etc/resolv.conf` and `/etc/hosts` (and a few directories like `/dev` `/sys` `/proc`)

- To Do: Provide cloudformtion example of SYS_PTRACE

2.  Add `SYS_PTRACE` capability (the only capability which you can add in Fargate) to the Task Definition
3.  Use Kaniko to perform the build (to a tar) - Note that Kaniko reserves a directory during the build, but using proot gets around this.
4.  As a part of the Dockerfile add any non standard library paths (such as Java may create) to ld.so.conf.d to the resulting image
5.  Using From the Go Container Registry project use the crane tool to upload to Docker repositories.

- Great source of info about al2 dockerfiles: `https://github.com/aws/aws-codebuild-docker-images/blob/master/al2/x86_64/standard/3.0/Dockerfile`

#### SSM/CloudWatch

- When fargate injects environment variables it does so to PID 1, so you may need to copy the variable name and value from PID 1 into the PID of Cloudwatch and SSM, in order to assume a role. `export ($strings /proc/1/environ | grep AWS_CONTAINER_CREDENTIALS_RELATIVE_URI )` and similar.
- `ENV RUN_IN_CONTAINER=True`
- If there are health check ipc errors: `rm -rf /var/lib/amazon/ssm/ipc`
- Worst case, you might need to treat the container like foriegn hybid infrastructure, and configure ssm accordingly.

#### Java library note

- In a Dockerfile, any non standard library path might need something like:

```
echo "/path/to/libs" > /etc/ld.so.conf.d/file.conf
ldconfig
```

- For Java it might be similar to:

```
echo $(dirname $(dirname $(which java | tr -d '[:space:]!')))"/lib"
```

#### AWS Corretto and OpenJDK do not have the same systemd requirement.

- AWS Corretto Headless via yum depends on systemd - and is not meant for containers. If you want to roll your own AL2 Java then refer to the Dockerfile provided by AWS for the correct yum repo. (Unlike OpenDK)

#### Save layers

- Create the image and then only copy the needed parts - for instance if a whole bunch of tools were installed to build a binary, but only the resulting binary is needed, or if the certificate store was manipulated using several commands, but only the final result is needed.

```
FROM mybase as firstthing
...
...
FROM myotherbase as secondthing
...
...
FROM scratch
COPY --from=firstthing / /first/
COPY --from=secondthing / /second/
```

- Caveat: Check that suid bit is still set correctly after copy.
