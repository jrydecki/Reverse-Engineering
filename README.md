# CS 479 Reverse Engineering at NMSU

## Summary
My name is Jacob Rydecki and this is my repository for CS 479 -- Reverse Engineering course at New Mexico State University for the Spring 2024 semester.
This repo contains all of the reports on reverse engineering malware samples from "Practical Malware Analysis".

## System Setup

### Startup
I started this assignment and setup with my MacOS laptop. I installed the Type-2 Hypervisor "VMware Fusion" and created a Windows 11 VM with it.

Throughout the following system and setup process I kept in mind the final goals and ideals of system & network isolation. I isolated my reverse-engineering system by creating setting up a Virtual Machine with VMware in order to keep the supervisor and the host separate in order
to prevent the accidental execution or spread of the malware I will be analyzing. All network updates, downloads, and operations were conducted 
right at the beginning of this course with the intent of never connected our network adapter again. By way of isolating this VM from the network, I am 
additionaly attempting to prevent the accdential spread of this malware. This will prevent any analyzed malware from reaching out in any way to the
network and other machines to spread.

Essentially, system & network isolation is used to be secure when analyzing and investigating malware and malware samples. If it is our goal
to understand how the malware is created and works, and to prevent the spread and proliferation of malware across the internet, we do not want to add to it and we should take simple security measures in order to ensure we don't contribute to it. Thus, our use of isolation.

I created the VM with the following steps:
- I downloaded the Windows 11 Installer ISO recommended by VMware and then booted by VM off that ISO. I used that image to install
Windows 11 onto my virtual hard disk created by my system. 
- After installing Windows, I started the VM without the network adapter connected in order create a local account on the VM, rather than linking 
the account with a Microsoft account. It turns out in Windows 11, I needed to also type Shift-F10 in order to bring up a CMD prompt and type
`OOBE\BYPASSNRO` in order to restart the setup process and create a local account during the setup.
- I then connected the network adapter to install updates to the VM, and for the following installations outlined below in [Software Installation](#software-installation)


At this point, I had my VM created, local VM account created and a fresh, updated system.

### Disabling Windows Defender

In order to disable Windows Defender, I followed the steps outlined in the Assignment 0 instructions including:
1. Booting into Safe Mode.
2. While in Safe Mode, changing 6 Registry Keys to disable Windows Defender from turning during system boot. To do this, I entered the following
commands into an administrator CMD Prompt:
    - ```
      reg add "HKLM\System\CurrentControlSet\Services\WdFilter" /v "Start" /t REG_DWORD /d "4" /f
      reg add "HKLM\System\CurrentControlSet\Services\WdNisDrv" /v "Start" /t REG_DWORD /d "4" /f
      reg add "HKLM\System\CurrentControlSet\Services\WdNisSvc" /v "Start" /t REG_DWORD /d "4" /f
      reg add "HKLM\System\CurrentControlSet\Services\WinDefend" /v "Start" /t REG_DWORD /d "4" /f
      reg add "HKLM\System\CurrentControlSet\Services\Sense" /v "Start" /t REG_DWORD /d "4" /f
      reg add "HKLM\System\CurrentControlSet\Services\WdBoot" /v "Start" /t REG_DWORD /d "4" /f
      ```
3. Booting back into normal Windows and disabling the scheduled tasks to turn Defender back on by doing the following:
    - Running `taskschd.msc` in CMD
    - Disabling the following scheduled tasks under `Microsoft` --> `Windows` --> `Windows Defender`:
      - Windows Defender Cache Maintenance
      - Windows Defender Cleanup
      - Windows Defender Scheduled Scan
      - Windows Defender Verification
4. I finally rebooted by VM, and ensured that when opening the Windows Security application it tells me "Threat service has stopped. Restart it now."


### Software Installation

I installed the following software onto my reverse engineering VM:
1. Visual Studio -- used to create, compile, and run C and C++ code.
2. Visual Studio Code -- used to open, view, and edit various different text-based files and source code files.
3. IDA Education -- used to decompile, investigate, and analyze binary and executables.
4. Flare VM -- used to install and manage many other tools that may be useful in the reverse engineering process.

After installing all of this software, I diconnected the network adapter from by VM in order to isolate the system from the network.