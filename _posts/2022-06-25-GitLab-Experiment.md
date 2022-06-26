---
layout: post
title: GitLab Experiment
subtitle: Why I set this up will forever remain shrouded in mystery
---

Sticking with the VM theme of recent times, I've decided to spin up my own instance of GitLab (just to see if I could). In future, I'll be trying to link my local GitLab instance with my currently registered domain and host all of my throw away projects/local documentation on there (Why? Because I can! After all this is probably less convenient than just pushing everything to GitHub). 

## Ubuntu 20.04 LTS Install

Installed Ubuntu Server 20.04LTS as a GEN1 HyperV VM using the AMD64 image. Selected OpenSSH Server to be iuncluded on install.
Credentials are:
`Username: gitlabadmin` `Password: gitlabadmin`

Installed the required tools (net-tools)

```bash
sudo apt-get install net-tools
```

## Gitlab Server Install

### Dependencies

```bash
sudo apt update
sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
# When installing postfix select "Internet Site" and enter your domain
sudo apt-get install -y postfix

# Configure Firewall
sudo ufw enable
sudo ufw allow http
sudo ufw allow https
sudo ufw allow Postfix
sudo ufw allow OpenSSH

# Check the status of UFW
sudo ufw status
```

### GitLab Server

```bash
# This installs GitLab EE, to install Gitlab CE, change the URL accordingly
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
sudo apt install gitlab-ee

# Change server configuration
sudo nano /etc/gitlab/gitlab.rb
# Change the external address to 'https://yourdomain' or 'http://yourip'

# Try login via Web to the specified IP
# Default root credentials can be found here
sudo nano /etc/gitlab/initial_root_password

# Make sure to change the root password on first time login
# Credentials: Username: root Password: gitlabadmin

# RECONFIGURE THE SERVER ANYTIME ANY CHANGES ARE MADE TO THE CONFIG FILE
sudo gitlab-ctl reconfigure
```

So far the GitLab server was configured with a static IP rather than a domain. User creation works, but probably needs extra configuration to ensure that a registering user has been given the right authorization to create his account (rather than any random person being able to access this GitLab instance once it's associated to a public domain).

**TODO** *Associate the GitLab instance to use a `roghi.dev` subdomain. WARNING: make sure the credentials for the underlying Ubuntu Server change once this happens.*
