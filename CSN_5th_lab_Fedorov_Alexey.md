# CSN. Lab 4. Networking Reconnaissance and Analysis

## Done by Fedorov Alexey

---

# Task 1. Theory

```

1. What is systemctl and init and systemd ?
2. what are the available Runlevel on linux?
3. what does the systemctl list-unit-files command does?
```

##  What is systemctl and init and systemd ?

![image](https://github.com/user-attachments/assets/178adbed-25c4-453a-9a12-2071bdf207bd)

`systemd` is a Linux initialization and unit manager. It controls how the system is booted and uses the `unit` abstraction to manage resources.

`systemd` initializes the system using `target` units. A target is usually a run level, but not all targets are run levels. The unit called `default.target` is the default run level configured in the system, and it is a symlink to `boot.target`.

![image](https://github.com/user-attachments/assets/a47c4084-a88c-45b1-a01e-00bbd5aed532)

A `unit` refers to any resource that the system knows how to operate and manage. There are several `unit` categories:

```
- .service: A service unit describes how to manage a service or application on the server. This will include how to start or stop the service, under which circumstances it should be automatically started, and the dependency and ordering information for related software.
- .socket: A socket unit file describes a network or IPC socket, or a FIFO buffer that systemd uses for socket-based activation. These always have an associated .service file that will be started when activity is seen on the socket that this unit defines.
- .device: A unit that describes a device that has been designated as needing systemd management by udev or the sysfs filesystem. Not all devices will have .device files. Some scenarios where .device units may be necessary are for ordering, mounting, and accessing the devices.
- .mount: This unit defines a mountpoint on the system to be managed by systemd. These are named after the mount path, with slashes changed to dashes. Entries within /etc/fstab can have units created automatically.
- .automount: An .automount unit configures a mountpoint that will be automatically mounted. These must be named after the mount point they refer to and must have a matching .mount unit to define the specifics of the mount.
- .swap: This unit describes swap space on the system. The name of these units must reflect the device or file path of the space.
- .target: A target unit is used to provide synchronization points for other units when booting up or changing states. They also can be used to bring the system to a new state. Other units specify their relation to targets to become tied to the target’s operations.
- .path: This unit defines a path that can be used for path-based activation. By default, a .service unit of the same base name will be started when the path reaches the specified state. This uses inotify to monitor the path for changes.
- .timer: A .timer unit defines a timer that will be managed by systemd, similar to a cron job for delayed or scheduled activation. A matching unit will be started when the timer is reached.
- .snapshot: A .snapshot unit is created automatically by the systemctl snapshot command. It allows you to reconstruct the current state of the system after making changes. Snapshots do not survive across sessions and are used to roll back temporary states.
- .slice: A .slice unit is associated with Linux Control Group nodes, allowing resources to be restricted or assigned to any processes associated with the slice. The name reflects its hierarchical position within the cgroup tree. Units are placed in certain slices by default depending on their type.
- .scope: Scope units are created automatically by systemd from information received from its bus interfaces. These are used to manage sets of system processes that are created externally.
```

`init` is the first process started during system boot. It is a daemon process that continues running until the system is shut down. `init` is the direct or indirect ancestor of all other processes and automatically adopts all orphaned processes.

![image](https://github.com/user-attachments/assets/4eb05a1c-6d20-48f3-b6d5-28a57a996553)

`systemctl` is a command-line interface to control `systemd` manually. It provides commands to list and manage `systemd` units. It can also manage the run levels of the system.

Example commands:

```
systemctl start sshd - Starts sshd.service

systemctl status sshd - Checks sshd.service status and also shows logs

systemctl list-units - Get list of all units that loaded to systemd

systemctl list-units --type=service - Get list of all service units
```

## What are the available Runlevel on linux?

Run levels in Linux represent different modes of operation that define which services and processes are running on the system.

There are 7 traditional run levels[4]
```
0 Halt - Shuts down the system.
1 Single-User (Rescue Mode) - A minimal mode, typically used for maintenance tasks. Only the root user can log in, and networking services are not started. This mode is used for troubleshooting and performing administrative tasks.
2 Multi-User - without Networking: In this run level, multiple users can log in, but networking is disabled. It's rarely used in modern systems.
3 Full Multi-User Mode with Networking - A non-graphical mode where multiple users can log in, and networking is enabled.
4 Unused - This run level is generally undefined and can be customized by the system administrator.
5 Multi-User Mode with Networking and GUI - Similar to run level 3 but with a graphical user interface (GUI) such as Wayland started. This is typically the default run level for desktop systems.
6 Reboot - Reboots the system.
```

Run levels can also be switched via `systemd`. `systemd` has several commands to perform this:

```
systemctl set-default multi-user.target - sets default run level

systemctl isolate graphical.target - switch run level of system for one time.
```

This `units` operate with run level targets[5].

- poweroff.target — system shutdown.
- rescue.target — recovery mode. In this mode, a minimal set of services is started, and networking is not enabled. It is used for system recovery purposes. It is similar to Safe Mode in Windows.
- multi-user.target — multi-user mode.
- graphical.target — multi-user mode with graphical support. However, if no graphical interface is installed, this target functions the same as multi-user.target. It is the default target in both Debian 11 and Ubuntu 22.04.
- reboot.target — system reboot.
- default.target — the mode that will be loaded by default. It is a symbolic link to one of the boot targets.

## What does the systemctl list-unit-files command does?

The full command:

```
sudo systemctl list-unit-files
```

Let's execute it:

![image](https://github.com/user-attachments/assets/44856af7-7ab2-4be7-9461-1e9b03434b73)

- `UNIT FILE` - name of unit file
- `STATE` - state of unit, related to file
- `VENDOR PRESET` - state of unit, related to file, that vendor setted up as default.

The command shows a list of unit files, their states, and vendor-preset states.

It also supports filtering unit files by type using the `--type=<type>` flag.

![image](https://github.com/user-attachments/assets/18e79d06-dbfa-436b-8061-9eee4387b9a0)

# Task 2. Creating Systemd service

```
Create a shell script that would write a text to a file echo "bla bla" > /tmp/test.txt and
then create a Systemd service that would this script. Here are the requirements:
1. It should run as your current user
2. the working directory should be set to the current user home directory
3. this service will only run after the network.target service
```

I created file lab5.sh

![image](https://github.com/user-attachments/assets/94999296-ad98-45d4-9eaa-933db094a1d8)

Then, I created a `systemd` service unit file.

![image](https://github.com/user-attachments/assets/a4f0c936-1380-4b86-a26c-8a8ec42d28df)

The file was successfully loaded into `systemd` and executed.

![image](https://github.com/user-attachments/assets/1f73213b-ba2d-4ae8-a585-d3c3ede1dcd8)

Let's follow the requirements.

## 2.1 It should run as your current user

To run the service as the current user, let's move the unit to `~/.config/systemd/user/`.

![image](https://github.com/user-attachments/assets/f7b1b95d-6284-4b68-9a55-a57ec656abd4)

To check if it works, I added an `$USER` echo.

![image](https://github.com/user-attachments/assets/133e9f30-dcf1-4684-98f1-7d8b1ddbfa8d)

Let's run it with the `--user` flag.

![image](https://github.com/user-attachments/assets/083e1103-e997-47b6-a014-801c7ccffa17)

It works!

## 2.2 The working directory should be set to the current user home directory

I specified working directory with `WorkingDirectory=/home/%u`[6].

![image](https://github.com/user-attachments/assets/b89719f4-85d2-4143-a60c-f682f32e494b)

Let's check how it works. I added `pwd` to script.

![image](https://github.com/user-attachments/assets/f3c7cbdf-6ed2-4bd7-9738-bf74a29283d3)

![image](https://github.com/user-attachments/assets/da8ffc0e-9bf9-4f91-bb43-37709d6d9223)

Works!

## 2.3 This service will only run after the network.target service

To run service only after network.target, I added `After: network.target`[6].

![image](https://github.com/user-attachments/assets/7a14e092-3e40-43e2-9c2d-b0212b6fdcb0)

I dont know how to check that it works, because of this I simply ran it.

![image](https://github.com/user-attachments/assets/3f51e978-de87-4165-8052-e56c025e069d)

It still works, good!

# Task 3. Install a web-server service

## Installation

I would like to install nginx. Let's do it.

```
apt install -y nginx
```

![image](https://github.com/user-attachments/assets/0ea1f798-7b46-4de8-a659-84d0e9884e10)

Verify that nginx installed.

```
dpkg -l | grep nginx
```

![image](https://github.com/user-attachments/assets/3cccb3c7-3dd3-44c7-bf6e-d29c7e7c9fdd)

![image](https://github.com/user-attachments/assets/e2d3d1ec-6650-4f9b-9d73-e501b29ff526)


## Systemd service unit implementation

I started with the `[Unit]` section. First, I added a description.

![image](https://github.com/user-attachments/assets/f4d3655f-cb3d-443b-a575-c4fc510c3d74)

The server must be executed after network initialization.

![image](https://github.com/user-attachments/assets/379da96e-ede3-4a07-9947-859dae1dbb88)

That's all for the `[Unit]` section. Now, let's describe the `[Service]` section. I started with the `Type`, which has (8 options)[https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html]. I will use `forking` because of the Nginx master-worker architecture.

![image](https://github.com/user-attachments/assets/11a39ca7-6ecc-4965-beb6-9f0fd56e4944)

The next step is to define the control commands. We have three main options to control the web server:

- **stop**
- **start**
- **reload**

Moreover, the configuration of the service requires testing before the server launch.

![image](https://github.com/user-attachments/assets/74ba386d-0b8d-41c3-8cd8-912ea6d4a93f)

flags[7]:
- `-t` - tests web server's configuration.
- `-q` - quiet launch. Suppresses non-error messages.
- `-g` - provides ability to setup global directives. `daemon on` for example
- `-s` - flag to send signal to main process. For example `reload` sends SIGHUP to master process and makes hot-reload of web server.

We can see that Nginx uses a `.pid` file. This is how Nginx knows which process to send signals to. `systemd` provides a feature to set the PID file using the `PIDFile` directive.

![image](https://github.com/user-attachments/assets/20145be2-1899-490c-a534-27e4801a5329)

The last thing I want to configure in the `[Service]` section is `KillMode`. This option controls how signals are sent to processes. Here are the available options[8]:

- `process` - kill only main process.
- `mixed` - kill main and children processes.
- `control-group` - kill the whole control group of service unit.
- `none` - do nothing.

I use the `mixed` option to kill the master Nginx process and the worker processes at the same time.

![image](https://github.com/user-attachments/assets/4b3b05c3-23be-403d-9ccd-58cb13a6666d)

The next section is `[Install]`. I will use it solely for one purpose — autostart. I can define the `WantedBy` directive there, which will execute the service at the chosen run level.

The system has a multi-user run level by default. I will use this target.

![image](https://github.com/user-attachments/assets/5e25e2e7-d9bb-40c4-84d6-ad73f5259a43)

Now systemd unit is ready.

## Systemd unit testing

I loaded service unit file to systemd.

![image](https://github.com/user-attachments/assets/8ffd40ad-9a83-4edc-a8c5-b32519c14f38)

Tried to start nginx.

![image](https://github.com/user-attachments/assets/972de03a-55b0-468d-9ec9-0cbd1cabb02e)

It works!

Nginx stop.

![image](https://github.com/user-attachments/assets/cc6c081f-bdd2-416b-804e-3347a88c89fe)

Reload.

![image](https://github.com/user-attachments/assets/b2d7ea76-caa4-4a5f-ac84-a83b22231603)


# Task 4. Crontab

![image](https://github.com/user-attachments/assets/90ffa4f0-e87e-4eaa-ae75-482965e7a30e)

Cron is a classic daemon used to periodically execute tasks at a specific time. Regular energy support activities posted in crontab files and special directories[9].

Main cron configuration file, `/etc/crontab`.

The crontab table consists of 6 columns separated by spaces or tabs. The first five columns specify the execution time. After the time fields, the user from whom the command is run is indicated. All other characters in the string are interpreted as an executed command with its parameters. If the command sends some text to the standard output, this text is sent by e-mail to the user[9].

```
* * * * * <user> <command>
- - - - -
| | | | |
| | | | ----- week day (0—7)
| | | ------- month (1—12)
| | --------- daoy of month (1—31)
| ----------- hour (0—23)
------------- minute (0—59)
```

## Set the shell script that you have written in Task 2 to run in a crontab, configure the crontab to run every 30 minute on Wednesday

Let's try 2 ways to execute script in crontab:

1. `crontab -e` command
2. file in `/etc/cron.d/` directory

### Crontab -e

To run the script this way, the only things we need to do are run the command and specify the cron rule. Let’s do it[10].

`crontab -e`

![image](https://github.com/user-attachments/assets/4545dee9-ef44-45a0-9fe3-3117eed8b877)

The script will be executed every 30 minutes on Wednesday.

### /etc/cron.d

For this, we need to create a file with the rule in `/etc/cron.d`[10].

```
vi /etc/cron.d/run_script
```
![image](https://github.com/user-attachments/assets/a635290d-ba2a-4876-9f4b-9fab50131970)

The script will be executed every 30 minutes on Wednesday.

### Systemd timer

Cron is a good scheduler, but it has disadvantages:

- No built-in logging: It records the fact of command execution, but there is no access to the process's stdout.
- To stop a schedule, you have to delete the file or comment out the configuration in the main file.

In my opinion, there is a better built-in solution for time scheduling in Linux: `systemd` timer units. Let's configure one.

![image](https://github.com/user-attachments/assets/c23281b7-bc27-431e-bb4b-9bc9802b9670)

This is the way to schedule execution as described in the task.

Then, I loaded it in `systemd` and reloaded the daemon:

![image](https://github.com/user-attachments/assets/7fefc261-0d7d-4bc8-bb05-981a7df06849)

Timer requires to be started.

![image](https://github.com/user-attachments/assets/cbb79ee3-8b4c-427a-8875-70f0052c8221)

Let's check it.

![image](https://github.com/user-attachments/assets/03072dfc-d2d0-4ee2-9df6-11db18f52d7c)

Now we have a table with information about when the timer was executed.

I think it is harder to configure, but much better than cron, because we can control the behavior of the timer and check the activity logs.

# References

1. https://ru.wikipedia.org/wiki/Systemd
2. https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
3. https://wiki.archlinux.org/title/Init#:~:text=Init%20is%20the%20first%20process,automatically%20adopts%20all%20orphaned%20processes.
4. https://wiki.merionet.ru/articles/runlevel-v-linux-chto-eto-i-s-chem-edyat
5. https://sysadminium.ru/adm-serv-linux-systemd-target/
6. https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html
7. https://linux.die.net/man/8/nginx
8. https://www.freedesktop.org/software/systemd/man/latest/systemd.kill.html
9. https://ru.wikipedia.org/wiki/Cron
10. https://crontab.guru/
