# How to Connect to a Remote Docker Daemon


## Pre-Requisite:

- A Ubuntu 18.04 installed on one of VM instance
- Install Docker



## Create the directory to store the configuration file.

```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

## Create a new file to store the daemon options.

```
sudo nano /etc/systemd/system/docker.service.d/options.conf
```

## Now make it look like this and save the file when you're done:
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H unix:// -H tcp://0.0.0.0:2375

# Reload the systemd daemon.
sudo systemctl daemon-reload

# Restart Docker.
sudo systemctl restart docker
```

That’s going to let you continue to connect to the Docker daemon from within the VM thanks to -H unix://, but it also exposes the Docker Daemon with -H tcp://0.0.0.0:2375 so that anyone can connect to it over the non-encrypted port.

##  Configuring your dev box to connect to the remote Docker daemon:


If you want to set DOCKER_HOST by default so it always connects remotely you can export it in your ~/.bashrc file. Here’s an example of that as a 1 liner:

```
echo "export DOCKER_HOST=tcp://X.X.X.X:2375" >> ~/.bashrc && source ~/.bashrc
```

That just adds the export line to your .bashrc file so it’s available every time you open your terminal. The source command reloads your bash configuration so it takes effect now.

Congratulations, you’re now able to connect to a remote Docker daemon.


