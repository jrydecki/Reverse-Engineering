# Assignment 0 Report

## System Setup
I started this assignment and setup with my MacOS laptop. I installed the Type-2 Hypervisor "VMware Fusion" and created a Windows 11 VM with it.
I did this by downloading the Windows 11 Installer ISO recommended by VMware and then booted by VM off that ISO. I used that image to install
Windows 11 onto my virtual hard disk created by my system. After installing Windows, I started the VM without the network adapter connected in
order create a local account on the VM, rather than linking the account with a Microsoft account. It turns out in Windows 11, I needed to also 
type Shift-F10 in order to bring up a CMD prompt and type `OOBE\BYPASSNRO` in order to restart the setup process and create a local account during 
the setup.

At this point, I had my VM created, local VM account created, and I moved on to install updates and reconnect the network adapter for the next steps.