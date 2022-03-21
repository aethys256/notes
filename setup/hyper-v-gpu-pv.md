# Setup: Microsoft Hyper-V GPU ParaVirtualization (Partitioning)

## Introduction

These are my personal Hyper-V GPU-PV through GPU Partitioning notes. Feel free to pick-up whatever you might need.\
This only works for Enterprise, Pro, or Education editions of Windows 10 and 11.

## Enable virtualization in the motherboard

Refer to your motherboard manual.

## Enable Hyper-V

1) Open a PowerShell console as Administrator
2) Run the following command:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

3) Reboot

Cf. https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v

## Download Windows using Rufus

Rufus download: https://rufus.ie/en/\
Change `SELECT` to `DOWNLOAD` using arrow then click on `DOWNLOAD` to choose the right ISO.\
Close Rufus.

## Allow PowerShell scripts execution

1) Open a PowerShell console as Administrator
2) Run the following command:

```powershell
Set-ExecutionPolicy unrestricted
```

## Hyper-V Network Switch

You might want to use Bridged network instead of NAT.\
To do so, open `Hyper-V Manager` then `Virtual Switch Manager`.\
If you want to customize the MAC address that will be assigned, change it in `MAC Address Range`.\
Create an external switch, you can name it `Bridge-MB_Manufacter-NetworkCard_Manufacter`, for example: `Bridge-Asus-Intel`.
Make sure the network is the network card you want.

## GPU Drivers

Make sure you have the latest NVIDIA/AMD/Intel GPU drivers installed on the host.

## Easy-GPU-PV

We will use [Easy-GPU-PV](https://github.com/jamesstringerparsec/Easy-GPU-PV) scripts for the VM setup.\
Follow instructions from the [ReadMe](https://github.com/jamesstringerparsec/Easy-GPU-PV#instructions).

Here are my edited values for reference:

```powershell
VMName = "MercuryVM"
SourcePath = "C:\Users\Aethys\Downloads\Win10_21H2_EnglishInternational_x64.iso"
SizeBytes = 80GB
MemoryAmount = 12GB
CPUCores = 6
NetworkSwitch = "Bridge-Asus-Intel"
VHDPath = "C:\Users\Aethys\Documents\Virtual Machines\"
GPUName = "AUTO"
GPUResourceAllocationPercentage = 10
Username = "Mercury"
Password = "WHAT_YOU_WANT"
```

## Setup

When the RDC window open, before login to Parsec you can do a quick setup of the VM :

- Task Bar
- Network Sharing Options
- Keyboard Layout
- ...

We will use Parsec but through a Virtual Display Adapter so when we disconnect from Parsec (or Hyper-V RDP) we still have a monitor plugged.

- Quit Parsec and Open `C:\ProgramData\Parsec\config.txt`
- Remove `host_virtual_monitors = 1` and `host_privacy_mode = 1`
- Enable the "Microsoft Hyper-V Video" Display Adapter in the Device Manager
- Download `usbmidd_v2.zip` from https://www.amyuni.com/downloads/usbmmidd_v2.zip
- Extract and run `usbmidd.bat` as administrator
- Open `Display settings` (right-click on desktop) and change the resolution to what you want for the 2nd monitor. -ample: `1920x1080`
- Launch Parsec and log in then go to Settings
- On the `Host` tab, set the Resolution to `1920x1080` and the Bandwidth Limit to `50 Mbps`
- Shutdown the VM
- Edit Hyper-V VM Settings
- Remove the Media from the DVD Drive
- Define a Static MAC Address in the Network Adapter (Advanced Feature)
- Start the VM
- Connect using Parsec

## Applications

Install your applications and have fun.\
Do not forget to activate Windows.

## Update the Resource Allocation Percentage

1) Shutdown the VM
2) Open a PowerShell console as Administrator
3) Run the following command:

```powershell
# Update those values to match your need
[string]$VMName = "MercuryVM"
[decimal]$GPUResourceAllocationPercentage = 35

[float]$divider = [math]::round($(100 / $GPUResourceAllocationPercentage), 2)

Set-VMGpuPartitionAdapter -VMName $VMName -MinPartitionVRAM ([math]::round($(1000000000 / $divider))) -MaxPartitionVRAM ([math]::round($(1000000000 / $divider))) -OptimalPartitionVRAM ([math]::round($(1000000000 / $divider)))
Set-VMGPUPartitionAdapter -VMName $VMName -MinPartitionEncode ([math]::round($(18446744073709551615 / $divider))) -MaxPartitionEncode ([math]::round($(18446744073709551615 / $divider))) -OptimalPartitionEncode ([math]::round($(18446744073709551615 / $divider)))
Set-VMGpuPartitionAdapter -VMName $VMName -MinPartitionDecode ([math]::round($(1000000000 / $divider))) -MaxPartitionDecode ([math]::round($(1000000000 / $divider))) -OptimalPartitionDecode ([math]::round($(1000000000 / $divider)))
Set-VMGpuPartitionAdapter -VMName $VMName -MinPartitionCompute ([math]::round($(1000000000 / $divider))) -MaxPartitionCompute ([math]::round($(1000000000 / $divider))) -OptimalPartitionCompute ([math]::round($(1000000000 / $divider)))
```

4) You can check that everything was applied correctly by doing :

```powershell
Get-VMGpuPartitionAdapter -VMName $VMName -ErrorAction SilentlyContinue
```
