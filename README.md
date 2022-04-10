# Docker be careful

This README is designed to teach the basics of Docker security.

# Section A - Host security

Docker containers are designed under a Linux kernel. The kernel of my host would be the same as that of the container because is shared among containers, therefore, I must have all the updated software.

The most relevant purpose of this section is to isolate the users. Only those users with root permissions should be able to access docker, since docker itself accesses the kernel of our host.

The optimal thing is to create a group of users dedicated to this functionality and add the users who are going to launch containers.

- Create `docker` user group.
```
sudo groupadd dockerRunners
cat /etc/group | grep "docker"
```

- Add your `user` to `dockerRunners` group.
```
sudo usermod -a -G dockerRunners yourUser
grep 'dockerRunners' /etc/group
```

# Section B - Docker daemon security

The docker daemon runs as superuser so we must prevent users from manipulating or reading its configurations.

The file to edit is `/etc/docker/daemon.json` leaving the `debug` parameter as `false`.

```
{
    "debug": false
}
```

# Section C - Container security

La opcion `ulimit` allows you to define the files that can be loaded by containers.

```
docker run --ulimit nofile=512:512 --rm debian sh -c "unlimit -n"
```

Going into attacks, you can set the limit to spend CPU on the contents.

```
docker run -it --cpus=".5" ubuntu /bin/bash
```

## Privilegies

By default `root` is our user. We can change it with `-u uuid` flag. We can change it with next instruction but we need to give them user privilegies.

```
docker run -u 4400 alpine ls /root
```
