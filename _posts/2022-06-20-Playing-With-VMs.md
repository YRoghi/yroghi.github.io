---
layout: post
title: Playing with VMs
subtitle: Hello again!
---

A few days ago I decided to get back into working on things that would be useful in a real life context. Where is better to start than to create a proper workspace/lab environment to experienment and play around with. The initial stage of this project is going to be broken down into three parts:

***

### Networking Device

An virtual switch for the Windows environment (RoghiDev_WinSW) will be created with a /28 subnet to allow for 14 end devices on this network. This can be changed later.

An virtual switch for the Linux environment (RoghiDev_LSW) will be created with a /28 subnet to allow for 14 end devices on this network. This can also be changed later.

### Setting up a Windows Environment

The initial Windows environment will contain 3 machines:
- A Domain Controller (DC) to have some domain join action going on with some powershell scripting. This server will be running Windows Server R2 2022.
- A Lab Management PC that will be such that the Domain Controller is a static host on the network. This workstation will be running the latest version of Windows 11.
- A Windows client that will be domain joined and enable interaction with the DC. This workstation will be running the latest version of Windows 11.

### Setting up a Linux Environment

The initial Linux environment will contain a single Ubuntu server to get used to playing with Linux servers and using SSH via an outside client to manage it. This allows for the Ubuntu server to be used as a sandbox environment for any Linux related tasks.

***

Once all of the VMs are set up, some funky labs can be started and findings will be documented here, so stay tuned!