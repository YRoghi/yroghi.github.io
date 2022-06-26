---
layout: post
title: Playing with VMs
subtitle: Hello again!
---

A few days ago I decided to get back into working on things that would be useful in a real life context. Where is better to start than to create a proper workspace/lab environment to experienment and play around with. 

It is important to note that in any normal circumstance, publishing IP schemes for a home lab would be a *horrific* idea (from a security standpoint). With the above stated, only IPs that are in the Private IP space will be listed in this repo. Any publicly reachable IPs will be redacted.

The initial stage of this project is going to be broken down into three parts:

***

### Networking Device

A virtual switch with a /24 subnet will be created for my home network. Due to some end devices in this lab environment using DHCP and others using a Static IP, any change in the network will result in some VMs losing connectivity to the rest of the network. This is a problem to investigate later.

### Setting up a Windows Environment

The initial Windows environment will contain 3 machines:
- A Domain Controller (DC) to have some domain join action going on with some powershell scripting. This server will be running Windows Server R2 2022.
- A Lab Management PC that will be such that the Domain Controller is a static host on the network. This workstation will be running the latest version of Windows 11.
- A Windows client that will be domain joined and enable interaction with the DC. This workstation will be running the latest version of Windows 11.

### Setting up a Linux Environment

The initial Linux environment will contain a single Ubuntu server to get used to playing with Linux servers and using SSH via an outside client to manage it. This allows for the Ubuntu server to be used as a sandbox environment for any Linux related tasks.

***

Once all of the VMs are set up, some funky labs can be started and findings will be documented here, so stay tuned!