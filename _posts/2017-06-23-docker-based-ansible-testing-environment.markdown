---
layout: post
title:  "Docker-Based Ansible Testing Environment"
date:   2017-06-23 00:00:00
description: Using Docker to Test Ansible Playbooks
tags: SoftwareEngineering
---

### Background

During my first weeks at Cermati after leaving Bukalapak, I had a chance to play a bit with the company's deployment scripts. We're using Ansible playbooks to automate a big part of the ops tasks.

One thing that I noticed was that we're still mostly testing Ansible playbooks we made on a VPS, usually our staging server.

By the way, Cermati's [currently hiring](https://www.cermati.com/career). We're not running any bug bounty program though, and I'm not a security engineer at the present. But no worries, Bukalapak's still running their bug bounty program as usual.

### Drawbacks of VPS

There are a few drawbacks of using VPS compared to using virtualization and containers on the engineers' local workstations for testing the playbooks.

#### Limited Resource

To test our Ansible playbooks, we need to make sure nobody's using the server we're going to use. We can provide more VPS for testing purposes, but it might be too costly considering we have better choices at hand.

Using virtualization or containers on each engineer's local machine, we can set up as many testing environments as we need without paying extra fees for new VPS.

#### Mutable Environment

If the server used for testing is not reverted from its original clean state after we're running the Ansible playbook or doing something manually on the server, there's a good chance the Ansible playbook will be missing a few steps it should have included due to the one writing the playbook forgot to update it after doing something manually on the server.

Some cloud providers such as AWS let us to take snapshots of the machine, but it would require the playbook testers to be able to access the AWS control panel. Given that the ones who're writing and testing the playbooks might be parties that shouldn't have that much access to the system, it's not a wise choice to make.

Virtual machine software such as VirtualBox offers each of us the ability to make snapshots of the system, and Docker containers are going to refer to the image from which they're built every time they're started.

### Vagrant + VirtualBox vs Docker

When talking about it with a few other engineers, looks like some of our engineers set themselves their own local testing environment using Vagrant and VirtualBox. I tried setting up a testing environment myself using Vagrant and VirtualBox, but looks like it's not a very convenient choice after considering a few things.

#### BIOS Settings

Some hardware vendors disable virtualization technologies by default. To enable it, we need to go to the BIOS and enable it ourselves.

Setting Vagrant + VirtualBox as the standard setup for the testing environment would require us to guide new engineers to change their BIOS settings, which steps are different depending on the type of hardware they're using. Adding these steps to our onboarding guide, which is already simple enough to follow, would be a killjoy for new engineers and interns.

Docker containers can run just fine without any virtualization setting changes on BIOS, which is obvious because Docker's not a virtualization technology.

#### Snapshots

Keeping and managing VirtualBox snapshots might be a bit complicated compared to simply using Docker to keep the default clean state of the testing environment.

We don't need to bother with taking and loading snapshots when using Docker. All we need is knowing how to run a new Docker container straight from its image.

```
$ docker run -ti our/testing-image
```

Lots of love to our friends at [Docker](https://www.docker.io).

#### Resource Usage

When using VirtualBox, we have a full-fledged machine running on our system. Considering that our standard machines doesn't have too much memory to spare, running a full-fledged virtual machine might not be a wise choice.

So another win for Docker in this regard.

### Getting Docker to Work

Of course, being a containerization technology instead of virtualization technology, Docker behaves differently than actual machines in one way or another. Some of these quirks broke our Ansible playbooks on the testing environment. Those same playbooks work just fine on virtual machines.

Docker containers seems to be prohibited from performing several functions regarding host configurations. The containers will raise errors when trying to edit the hostname, or trying to replace certain configuration files with another.

To solve this problem and let the Ansible playbooks work as they're supposed to be in the real servers, I did some sandboxing to the quirky functions I found during the setup.

The `hostname` and `mv` command's capabilities are limited in the Docker container. So I need to make a workaround for both of them to keep the system working.

```bash
#!/bin/bash
# ./sandbox/hostname

if [[ $# -ne 0 ]]
then
  echo hostname is disabled
else
  hostname.unix
fi
```

```bash
#!/bin/bash
# ./sandbox/mv

if [[ ($# -ne 3 && $2 = '/etc/hosts') ]]
then
  echo mv is disabled for $2, using cat and rm instead
  cat $1 > $2
  rm $1
else
  mv.unix $1 $2 2>&1 >/dev/null | sed 's/.unix//g'
fi
```

A function we use on our Ansible script calls the [replace module](http://docs.ansible.com/ansible/replace_module.html), which broke the script. The supposed behavior seems similar with `mv`, but it calls Python standard library's `os.rename()` function instead.

```python
# ./sandbox/os.py
# This will patch the os.rename() function call from Python standard library

def rename(old, new):
  system("mv " + old + " " + new)
```

Prepare our entrypoint file, make sure SSH service is started. We're going to drop a shell so we can interactively check the changes inside the container made by the Ansible playbooks we have.

```bash
#!/bin/bash
# ./entrypoint.sh

# set root's password to 'secret'
echo -e "secret\nsecret" | passwd

# set other users' password to 'secret'
for user in $(ls /home)
do
  usermod -aG sudo $user
  echo -e "secret\nsecret" | passwd $user
done

service ssh start
/bin/bash
```

After we prepared these sandboxing functions, now we can build the sample Docker container.

```
FROM ubuntu:14.04

RUN apt-get update \
    && apt-get install -y \
    openssh-server

RUN useradd -m test_user

# Preparing sandbox environment
COPY ./sandbox /root/sandbox
RUN cp /bin/hostname /bin/hostname.unix \
    && cat /root/sandbox/hostname > /bin/hostname \
    && cp /bin/mv /bin/mv.unix \
    && cat /root/sandbox/mv > /bin/mv \
    && cat /root/sandbox/os.py >> /usr/lib/python2.7/os.py

COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```

Now, build the container.

```
$ docker build -t our/testing-image -f Dockerfile .
```

Run the container.

```
$ docker run -ti our/testing-image
```

We can SSH to the container using the credentials of `test_user`. Now we can run our Ansible playbooks on the container using SSH.

You can try using other OS images and setups. You can also update the sandboxed functions to adapt with your needs.