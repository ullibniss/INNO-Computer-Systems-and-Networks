# CSN. Lab 4. Networking Reconnaissance and Analysis

## Done by Fedorov Alexey

---

# Task 1. Theory

```

1. What is systemctl and init and systemd ?
2. what are the available Runlevel on linux?
3. what does the systemctl list-unit-files command does?
```

# 1.1

![image](https://github.com/user-attachments/assets/178adbed-25c4-453a-9a12-2071bdf207bd)

Systemd - is a linux initialization and units manager. It controls how system is booted.

Systemctl - is a command line interface to control systemd manually.

Run levels - is system boot levels. Every level setups system it's way.


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

Then I created systemd service unit file

![image](https://github.com/user-attachments/assets/a4f0c936-1380-4b86-a26c-8a8ec42d28df)

File succesfully loaded to systemd and executed

![image](https://github.com/user-attachments/assets/1f73213b-ba2d-4ae8-a585-d3c3ede1dcd8)

Let's follow requirements.

## 2.1 It should run as your current user

To run service as current user, let's replace unit to `~/.config/systemd/user/`

![image](https://github.com/user-attachments/assets/f7b1b95d-6284-4b68-9a55-a57ec656abd4)

To check whether it works, I added $USER echo.

![image](https://github.com/user-attachments/assets/133e9f30-dcf1-4684-98f1-7d8b1ddbfa8d)

Let's run it with `--user` flag.

![image](https://github.com/user-attachments/assets/083e1103-e997-47b6-a014-801c7ccffa17)

Works!

## 2.2 The working directory should be set to the current user home directory

I specified working directory with `WorkingDirectory=/home/%u`

![image](https://github.com/user-attachments/assets/b89719f4-85d2-4143-a60c-f682f32e494b)

Let's check how it works. I added `pwd` to script.

![image](https://github.com/user-attachments/assets/f3c7cbdf-6ed2-4bd7-9738-bf74a29283d3)

![image](https://github.com/user-attachments/assets/da8ffc0e-9bf9-4f91-bb43-37709d6d9223)

Works!

## 2.3 This service will only run after the network.target service

To run service only after network.target, I added `After: network.target`.

![image](https://github.com/user-attachments/assets/7a14e092-3e40-43e2-9c2d-b0212b6fdcb0)

I dont know how to check that it works, because of this I simply will run it.

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

I started with `[Unit section]`. Firstly I added description.

![image](https://github.com/user-attachments/assets/f4d3655f-cb3d-443b-a575-c4fc510c3d74)

Server must be executed after network initialization.

![image](https://github.com/user-attachments/assets/379da96e-ede3-4a07-9947-859dae1dbb88)

That all with `[Unit]` section. Let's describe `[Service]`. I started with Type, this field has (8 options)[https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html]. I will use `forking`, because of nginx master-worker architecture.

![image](https://github.com/user-attachments/assets/11a39ca7-6ecc-4965-beb6-9f0fd56e4944)

Next step is to define control commands. We have three main options to control web server:

- stop
- start
- reload

Moreover, configuration of service required test before server launch.

![image](https://github.com/user-attachments/assets/74ba386d-0b8d-41c3-8cd8-912ea6d4a93f)

flags:
- `-t` - tests web server's configuration.
- `-q` - quiet launch. Suppresses non-error messages.
- `-g` - provides ability to setup global directives. `daemon on` for example
- `-s` - flag to send signal to main process. For example `reload` sends SIGHUP to master process and makes hot-reload of web server.

We can see that nginx uses .pid file. This is the way nginx knows what process to send signals. Systemd provides feature to set pid file via `PIDfile` line.

![image](https://github.com/user-attachments/assets/20145be2-1899-490c-a534-27e4801a5329)

The last thing I want to configure in `[Service]` section is KillMode. This function controls how signals will be sent to processes, here is options:

- `process` - kill only main process.
- `mixed` - kill main and children processes.
- `control-group` - kill the whole control group of service unit.
- `none` - do nothing.

I use `mixed` option, to kill master nginx process and workers at the same time.

![image](https://github.com/user-attachments/assets/4b3b05c3-23be-403d-9ccd-58cb13a6666d)

Next section is `[Install]`. I will use it only for one purpose - autostart. I can define there `WantedBy` directive that will execute service on chosen run-level. 

System has multi-user run level by default. I will use this target.

![image](https://github.com/user-attachments/assets/5e25e2e7-d9bb-40c4-84d6-ad73f5259a43)

Now systemd unit is ready.

## Systemd unit testing



# Task 4. Crontab

Let's try 2 ways to execute script in crontab:

1. `crontab -e` command
2. file in `/etc/cron.d/` directory

## Crontab -e

To run script this way, the only things we need to run command and scpecify cron rule. Let's do it.

`crontab -e`

![image](https://github.com/user-attachments/assets/4545dee9-ef44-45a0-9fe3-3117eed8b877)

Script will be executed every 30 minutes at Wednesday.


## /etc/cron.d

For this way we need to create file with rule in /etc/cron.d

```
vi /etc/cron.d/run_script
```
![image](https://github.com/user-attachments/assets/a635290d-ba2a-4876-9f4b-9fab50131970)

Script will be executed every 30 minutes at Wednesday.
