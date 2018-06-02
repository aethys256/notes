# Dev Upgrade Utils (macOS)

## Introduction
These are my personal macOS hackintosh notes for the Intel X299 platform. Feel free to pick-up whatever you might need.
Before starting, I recommend you to read [this thread on tonymacx86](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/).
Do remember that this is related to my setup and you might want to adapt based on your hardware.
On a side note, I choosed to disable the built-in Bluetooth & Wi-Fi adapter since I wasn't using them in macOS I won't use them in Windows.
The intended setup is to have both macOS and Windows bootable (dual boot thanks to Clover).
The reason was to have a nice setup for development mainly, while still be able to run Windows-only application natively if needed (some games / softwares).

## PC Setup

### Hardware
>**Motherboard** - Asus ROG Rampage VI APEX
>**CPU** - Intel Core i9-7980 XE @ 4,6 GHz
>**RAM** - G.SKILL F4-3600C17Q-64GTZR 64 GB (4 x 16 GB) Trident Z RGB Series DDR4 3600 MHz
>**GPU** - EVGA GeForce GTX 1080 FTW2 GAMING 8 GB

### Usage
Mainly development and gaming.
It is also on this machine that the simulations of [HeroDamage](https://www.herodamage.com/) are run (inside a Linux VM).
Hence the workstation choice.

## Prepare the BIOS

### Firmware
Flash the latest firmware available, as of May 2018, this is the 1301 for me.

### Stock -> Default
Then load the `optimized defaults` and save it as an O.C. profile.
Then we disable the fancy RGB LEDs, configure the fans and save it as `default` profile.
```diff
< Onboard LED [Enabled]
> Onboard LED [Disabled]

< When system is in working state [On]
< When system is in sleep, hibernate or soft off states [On]
> When system is in working state [Off]
> When system is in sleep, hibernate or soft off states [Off]

< CPU Q-Fan Control [Auto]
> CPU Q-Fan Control [PWM Mode]

< CPU Fan Profile [Standard]
< Water Pump+ Control [Disabled]
> CPU Fan Profile [Manual]
> CPU Upper Temperature [60]
> CPU Fan Max. Duty Cycle (%) [100]
> CPU Middle Temperature [55]
> CPU Fan Middle. Duty Cycle (%) [70]
> CPU Lower Temperature [20]
> CPU Fan Min. Duty Cycle (%) [20]
> Water Pump+ Control [PWM Mode]
> Water Pump+ Q-Fan Source [CPU]
> Water Pump+ Upper Temperature [60]
> Water Pump+ Max. Duty Cycle (%) [100]
> Water Pump+ Middle Temperature [55]
> Water Pump+ Middle. Duty Cycle (%) [45]
> Water Pump+ Lower Temperature [20]
> Water Pump+ Min. Duty Cycle (%) [30]

< Chassis Fan 1 Q-Fan Control [Auto]
> Chassis Fan 1 Q-Fan Control [PWM Mode]

< Chassis Fan 1 Profile [Standard]
< Chassis Fan 2 Q-Fan Control [Auto]
> Chassis Fan 1 Profile [Manual]
> Chassis Fan 1 Upper Temperature [60]
> Chassis Fan 1 Max. Duty Cycle (%) [100]
> Chassis Fan 1 Middle Temperature [55]
> Chassis Fan 1 Middle. Duty Cycle (%) [70]
> Chassis Fan 1 Lower Temperature [20]
> Chassis Fan 1 Min. Duty Cycle (%) [20]
> Chassis Fan 2 Q-Fan Control [PWM Mode]

< Chassis Fan 2 Profile [Standard]
< Chassis Fan 3 Q-Fan Control [Auto]
> Chassis Fan 2 Profile [Manual]
> Chassis Fan 2 Upper Temperature [60]
> Chassis Fan 2 Max. Duty Cycle (%) [100]
> Chassis Fan 2 Middle Temperature [55]
> Chassis Fan 2 Middle. Duty Cycle (%) [70]
> Chassis Fan 2 Lower Temperature [20]
> Chassis Fan 2 Min. Duty Cycle (%) [20]
> Chassis Fan 3 Q-Fan Control [PWM Mode]


< Chassis Fan 3 Profile [Standard]
> Chassis Fan 3 Profile [Manual]
> Chassis Fan 3 Upper Temperature [60]
> Chassis Fan 3 Max. Duty Cycle (%) [100]
> Chassis Fan 3 Middle Temperature [55]
> Chassis Fan 3 Middle. Duty Cycle (%) [70]
> Chassis Fan 3 Lower Temperature [20]
> Chassis Fan 3 Min. Duty Cycle (%) [20]

< POST Delay Time [3 sec]
> POST Delay Time [1 sec]
```

### Default -> XMP
We turn on XMP according to the RAM installed.
```diff
< Ai Overclock Tuner [Auto]
< ASUS MultiCore Enhancement [Auto]
> Ai Overclock Tuner [XMP]
> XMP [XMP DDR4-3603 17-19-19-39-1.350V]
> CPU Strap [100]
> BCLK Frequency [100.0000]
> ASUS MultiCore Enhancement [Disabled]

< CPU Core Ratio [By Core Usage]
> CPU Core Ratio [Auto]

< DRAM Frequency [Auto]
> DRAM Frequency [DDR4-3600MHz]

< CPU Input Voltage [Auto]
< DRAM Voltage(CHA, CHB) [Auto]
< DRAM Voltage(CHC, CHD) [Auto]
> DRAM Voltage(CHA, CHB) [1.3500]
> DRAM Voltage(CHC, CHD) [1.3500]

< Maximus Tweak [Auto]
> Maximus Tweak [Mode 1]

< DRAM CAS# Latency [Auto]
< DRAM RAS# to CAS# Delay [Auto]
< DRAM RAS# PRE Time [Auto]
< DRAM RAS# ACT Time [Auto]
> DRAM CAS# Latency [17]
> DRAM RAS# to CAS# Delay [19]
> DRAM RAS# PRE Time [19]
> DRAM RAS# ACT Time [39]

< MSR Lock Control [Enabled]
> MSR Lock Control [Disabled]
```

### XMP -> macOS
We need to tweak these settings for macOS.
```diff
< CPU SVID Support [Auto]
> CPU SVID Support [Enabled]

< Autonomous Core C-State [Auto]
< Intel(R) Speed Shift Technology [Auto]
< MFC Mode Override [MFC Driver Override]
> Autonomous Core C-State [Enabled]
> Enhanced Halt State (C1E) [Enabled]
> CPU C6 report [Enabled]
> Package C State [C6(non Retention) state]
> Intel(R) Speed Shift Technology [Enabled]
> MFC Mode Override [OS Native Support]

< Intel® VT for Directed I/O (VT-d) [Enabled]
> Intel® VT for Directed I/O (VT-d) [Disabled]

< Wi-Fi 802.11ac Controller [Enabled]
< Bluetooth Controller [Enabled]
> Wi-Fi 802.11ac Controller [Disabled]
> Bluetooth Controller [Disabled]

< Fast Boot [Enabled]
< Next Boot after AC Power Loss [Normal Boot]
> Fast Boot [Disabled]

< Boot up NumLock State [Enabled]
> Boot up NumLock State [Disabled]

< Launch CSM [Enabled]
< Boot Device Control [UEFI and Legacy OPROM]
< Boot from Network Devices [Legacy only]
< Boot from Storage Devices [Legacy only]
< Boot from PCI-E/PCI Expansion Devices [Legacy only]
> Launch CSM [Disabled]
```


### CSM
Enable the CSM if needed (might be the case if the Video Card doesn't support EFI boot).
```diff
< Launch CSM [Disabled]
> Launch CSM [Enabled]
> Boot Device Control [UEFI only]
> Boot from Network Devices [UEFI driver first]
> Boot from Storage Devices [UEFI driver first]
> Boot from PCI-E/PCI Expansion Devices [UEFI driver first]
```

### Asus Multi-Core Enhancement
Enable it if you want more performance and your CPU can handle that. I set it to 4.6 GHz since I don't want to run the i9-7980XE too hot.
```diff
< ASUS MultiCore Enhancement [Disabled]
< AVX Instruction Core Ratio Negative Offset [Auto]
< AVX-512 Instruction Core Ratio Negative Offset [Auto]
< CPU Core Ratio [Auto]
> ASUS MultiCore Enhancement [Auto]
> AVX Instruction Core Ratio Negative Offset [3]
> AVX-512 Instruction Core Ratio Negative Offset [2]
> CPU Core Ratio [Sync All Cores]
> ALL-Core Ratio Limit [46]

< Maximus Tweak [Mode 1]
> Maximus Tweak [Auto]
```

## Prepare the EFI partition
Considering I already did the tests to make it stable for my configuration, I did more steps than what kgp (thread owner) recommends for the first boot.
You should stick to the thread if it's your first time, use this page as a double check only (my procedure is slightly different but equivalent).
To start, unzip the EFI folder from kgp.

### config.plist
Make a backup of the initial config.plist given by kgp and configure it according to the .diff below using Clover Configurator.
```diff
--- config_kgp.plist
+++ config.plist
 <?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
 <plist version="1.0">
 <dict>
 	<key>ACPI</key>
 	<dict>
 		<key>DSDT</key>
 		<dict>
 			<key>Debug</key>
 			<false/>
 			<key>DropOEM_DSM</key>
 			<false/>
 			<key>Name</key>
 			<string>DSDT.aml</string>
 			<key>Patches</key>
 			<array>
 				<dict>
 					<key>Comment</key>
 					<string>CAVS -&gt; HDEF</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					Q0FWUw==
 					</data>
 					<key>Replace</key>
 					<data>
 					SERFRg==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>PC00 -&gt; PCI0</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					UEMwMA==
 					</data>
 					<key>Replace</key>
 					<data>
 					UENJMA==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>SL05 -&gt; GFX0</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					U0wwNQ==
 					</data>
 					<key>Replace</key>
 					<data>
 					R0ZYMA==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>_OSI -&gt; XOSI</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					X09TSQ==
 					</data>
 					<key>Replace</key>
 					<data>
 					WE9TSQ==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>EC0_ -&gt; EC__</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					RUMwXw==
 					</data>
 					<key>Replace</key>
 					<data>
 					RUNfXw==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>H_EC -&gt; EC__</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					SF9FQw==
 					</data>
 					<key>Replace</key>
 					<data>
 					RUNfXw==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>GBE1 -&gt; ETH0</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					R0JFMQ==
 					</data>
 					<key>Replace</key>
 					<data>
 					RVRIMA==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>D0A4 -&gt; ETH1</string>
 					<key>Disabled</key>
 					<true/>
 					<key>Find</key>
 					<data>
 					RDBBNA==
 					</data>
 					<key>Replace</key>
 					<data>
 					RVRIMQ==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>HEC1 -&gt; IMEI</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					SEVDMQ==
 					</data>
 					<key>Replace</key>
 					<data>
 					SU1FSQ==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>IDER-&gt;MEID</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					SURFUg==
 					</data>
 					<key>Replace</key>
 					<data>
 					TUVJRA==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>PMC1 -&gt; PMCR</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					UE1DMQ==
 					</data>
 					<key>Replace</key>
 					<data>
 					UE1DUg==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>LPC0 -&gt; LPCB</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					TFBDMA==
 					</data>
 					<key>Replace</key>
 					<data>
 					TFBDQg==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>FPU_ -&gt; MATH</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					RlBVXw==
 					</data>
 					<key>Replace</key>
 					<data>
 					TUFUSA==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>TMR_ -&gt; TIMR</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					VE1SXw==
 					</data>
 					<key>Replace</key>
 					<data>
 					VElNUg==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>PIC_ -&gt; IPIC</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					UElDXw==
 					</data>
 					<key>Replace</key>
 					<data>
 					SVBJQw==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>SLOC -&gt; ARPT</string>
 					<key>Disabled</key>
 					<true/>
 					<key>Find</key>
 					<data>
 					U0wwQw==
 					</data>
 					<key>Replace</key>
 					<data>
 					QVJQVA==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>_DSM -&gt; XDSM</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					X0RTTQ==
 					</data>
 					<key>Replace</key>
 					<data>
 					WERTTQ==
 					</data>
 				</dict>
 				<dict>
 					<key>Comment</key>
 					<string>SMBS._ADR -&gt; XSBU.XADR</string>
 					<key>Disabled</key>
-					<true/>
+					<false/>
 					<key>Find</key>
 					<data>
 					U01CUwhfQURS
 					</data>
 					<key>Replace</key>
 					<data>
 					WFNCVQhYQURS
 					</data>
 				</dict>
 			</array>
 			<key>ReuseFFFF</key>
 			<false/>
 		</dict>
 		<key>DropTables</key>
 		<array>
 			<dict>
 				<key>Signature</key>
 				<string>DMAR</string>
 			</dict>
 		</array>
 		<key>SSDT</key>
 		<dict>
 			<key>DropOem</key>
 			<false/>
 			<key>Generate</key>
 			<dict>
 				<key>CStates</key>
 				<false/>
 				<key>PStates</key>
 				<false/>
 				<key>PluginType</key>
 				<true/>
 			</dict>
 		</dict>
 	</dict>
 	<key>Boot</key>
 	<dict>
 		<key>Arguments</key>
-		<string>-v darkwake=0 keepsyms=1 debug=0x100</string>
+		<string>darkwake=0</string>
 		<key>Debug</key>
 		<false/>
 		<key>DefaultLoader</key>
 		<string>BOOTX64.efi</string>
 		<key>DefaultVolume</key>
 		<string>LastBootedVolume</string>
 		<key>Legacy</key>
 		<string>PBR</string>
 		<key>Secure</key>
 		<false/>
 		<key>Timeout</key>
-		<integer>5</integer>
+		<integer>3</integer>
 		<key>XMPDetection</key>
 		<string>Yes</string>
 	</dict>
 	<key>CPU</key>
 	<dict>
 		<key>Type</key>
 		<string>0x</string>
 		<key>UseARTFrequency</key>
 		<false/>
 	</dict>
 	<key>Devices</key>
 	<dict>
 		<key>Audio</key>
 		<dict>
 			<key>Inject</key>
 			<string>7</string>
 		</dict>
 		<key>USB</key>
 		<dict>
 			<key>FixOwnership</key>
 			<false/>
 			<key>Inject</key>
 			<false/>
 		</dict>
 	</dict>
 	<key>GUI</key>
 	<dict>
 		<key>Hide</key>
 		<array>
 			<string>Preboot</string>
 			<string>Recovery</string>
 			<string>Windows</string>
 		</array>
 		<key>Language</key>
 		<string>en:0</string>
 		<key>Mouse</key>
 		<dict>
 			<key>DoubleClick</key>
 			<integer>500</integer>
 			<key>Enabled</key>
 			<true/>
 			<key>Mirror</key>
 			<false/>
 			<key>Speed</key>
 			<integer>8</integer>
 		</dict>
 		<key>Scan</key>
 		<dict>
 			<key>Entries</key>
 			<true/>
 			<key>Linux</key>
 			<false/>
 			<key>Tool</key>
 			<false/>
 		</dict>
 		<key>ScreenResolution</key>
-		<string>3840x1600</string>
+		<string>1920x1080</string>
 		<key>ShowOptimus</key>
 		<true/>
 		<key>Theme</key>
-		<string>Beauty</string>
+		<string>HighSierra</string>
 	</dict>
 	<key>Graphics</key>
 	<dict>
 		<key>Inject</key>
 		<dict>
 			<key>ATI</key>
 			<false/>
 			<key>Intel</key>
 			<false/>
 			<key>NVidia</key>
 			<false/>
 		</dict>
 		<key>NvidiaSingle</key>
 		<false/>
 	</dict>
 	<key>KernelAndKextPatches</key>
 	<dict>
 		<key>AppleIntelCPUPM</key>
 		<false/>
 		<key>AppleRTC</key>
 		<false/>
 		<key>Debug</key>
 		<false/>
 		<key>DellSMBIOSPatch</key>
 		<false/>
 		<key>KernelCpu</key>
 		<false/>
 		<key>KernelLapic</key>
 		<false/>
 		<key>KernelPm</key>
 		<false/>
 		<key>KernelToPatch</key>
 		<array>
 			<dict>
 				<key>Comment</key>
 				<string>xcpm_core_scope_msrs © Pike R. Alpha</string>
 				<key>Disabled</key>
 				<true/>
 				<key>Find</key>
 				<data>
 				vgMAAAAx0uhy/P//
 				</data>
 				<key>Replace</key>
 				<data>
 				vgMAAAAx0pCQkJCQ
 				</data>
 			</dict>
 		</array>
 		<key>KernelXCPM</key>
 		<false/>
 		<key>KextsToPatch</key>
 		<array>
 			<dict>
 				<key>Comment</key>
 				<string>SSD Trim Enabler</string>
 				<key>Disabled</key>
 				<false/>
 				<key>Find</key>
 				<data>
 				QVBQTEUgU1NEAA==
 				</data>
 				<key>InfoPlistPatch</key>
 				<false/>
 				<key>Name</key>
 				<string>IOAHCIBlockStorage</string>
 				<key>Replace</key>
 				<data>
 				AAAAAAAAAAAAAA==
 				</data>
 			</dict>
 			<dict>
 				<key>Comment</key>
 				<string>USB Port Limit Patch ©PMHeart</string>
 				<key>Disabled</key>
 				<false/>
 				<key>Find</key>
 				<data>
 				g32UDw+DlwQAAA==
 				</data>
 				<key>InfoPlistPatch</key>
 				<false/>
 				<key>Name</key>
 				<string>AppleUSBXHCI</string>
 				<key>Replace</key>
 				<data>
 				g32UGpCQkJCQkA==
 				</data>
 			</dict>
 			<dict>
 				<key>Comment</key>
 				<string>F1/F2 Key Patch 1 ©Wern</string>
 				<key>Disabled</key>
 				<true/>
 				<key>Find</key>
 				<data>
 				MHgwMDA3MDAzYSwweGZmMDEwMDIx
 				</data>
 				<key>InfoPlistPatch</key>
 				<false/>
 				<key>Name</key>
 				<string>com.apple.driver.AppleHIDKeyboard</string>
 				<key>Replace</key>
 				<data>
 				MHgwMDA3MDAzYSwweDAwMDcwMDNh
 				</data>
 			</dict>
 			<dict>
 				<key>Comment</key>
 				<string>F1/F2 Key Patch 2 ©Wern</string>
 				<key>Disabled</key>
 				<true/>
 				<key>Find</key>
 				<data>
 				MHgwMDA3MDAzYiwweGZmMDEwMDIw
 				</data>
 				<key>InfoPlistPatch</key>
 				<false/>
 				<key>Name</key>
 				<string>com.apple.driver.AppleHIDKeyboard</string>
 				<key>Replace</key>
 				<data>
 				MHgwMDA3MDAzYiwweDAwMDcwMDNi
 				</data>
 			</dict>
 			<dict>
 				<key>Comment</key>
 				<string>FredWst DP/HDMI patch</string>
 				<key>Disabled</key>
 				<false/>
 				<key>Find</key>
 				<data>
 				3hALDg==
 				</data>
 				<key>InfoPlistPatch</key>
 				<false/>
 				<key>Name</key>
 				<string>com.apple.driver.AppleHDAController</string>
 				<key>Replace</key>
 				<data>
 				3hDvEA==
 				</data>
 			</dict>
 		</array>
 	</dict>
 	<key>RtVariables</key>
 	<dict>
 		<key>BooterConfig</key>
 		<string>0x28</string>
 		<key>CsrActiveConfig</key>
 		<string>0x67</string>
 		<key>ROM</key>
 		<string>UseMacAddr0</string>
 	</dict>
 	<key>SMBIOS</key>
 	<dict>
 		<key>BiosReleaseDate</key>
 		<string>12/08/2017</string>
 		<key>BiosVendor</key>
 		<string>Apple Inc.</string>
 		<key>BiosVersion</key>
 		<string>IMP11.88Z.0064.B30.1712081714</string>
 		<key>Board-ID</key>
 		<string>Mac-7BA5B2D9E42DDD94</string>
 		<key>BoardManufacturer</key>
 		<string>Apple Inc.</string>
 		<key>BoardType</key>
 		<integer>10</integer>
 		<key>BoardVersion</key>
 		<string>1.0</string>
 		<key>ChassisAssetTag</key>
 		<string>iMacPro-Aluminum</string>
 		<key>ChassisManufacturer</key>
 		<string>Apple Inc.</string>
 		<key>ChassisType</key>
 		<string>0x09</string>
 		<key>Family</key>
 		<string>iMac Pro</string>
 		<key>FirmwareFeatures</key>
 		<string>0xFD8FF53E</string>
 		<key>FirmwareFeaturesMask</key>
 		<string>0xFF9FFF3F</string>
 		<key>LocationInChassis</key>
 		<string>Part Component</string>
 		<key>Manufacturer</key>
 		<string>Apple Inc.</string>
 		<key>Mobile</key>
 		<false/>
 		<key>PlatformFeature</key>
 		<string>0xFFFF</string>
 		<key>ProductName</key>
 		<string>iMacPro1,1</string>
 		<key>Trust</key>
 		<true/>
 		<key>Version</key>
 		<string>1.0</string>
 	</dict>
 	<key>SystemParameters</key>
 	<dict>
 		<key>InjectKexts</key>
 		<string>Yes</string>
 		<key>InjectSystemID</key>
 		<true/>
 	</dict>
 </dict>
 </plist>
```

In the SMBIOS section:
> **Serial number** - `Generate New` button.
> **SmUUID** - Open the `Terminal` and type `uuidgen` then copy/paste it.

### TSCAdjustReset.kext
Build `TSCAdjustReset.kext`.
Open `Info.plist` (Show Package Contents -> Contents).
Adjust `IOCPUNumber` in the (`35` for i9-7980XE).
Copy to `/EFI/CLOVER/kexts/Other/`.

### Other kexts

**Audio**
> `CodecCommander.kext`, `AppleALC.kext` and `Lilu.kext` copied to `/EFI/CLOVER/kexts/Other/`

Download:
* https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/
* https://github.com/vit9696/AppleALC/releases
* https://github.com/vit9696/Lilu/releases

**Hardware Sensors**
> `ACPISensors.kext`, `CPUSensors.kext`, `GPUSensors.kext` and `LPCSensors.kext` copied to `/EFI/CLOVER/kexts/Other/`

See kgp's thread to download these (`HW-Sensors-IF.zip`).

### ACPI patched

Copy `SSDT-X299-iMacPro-Customized.aml` to `/EFI/CLOVER/ACPI/patched/`.
See the SSDT folder for the diff between kgp's SSDT and this one, some parts were pruned then the audio card was renamed.

## macOS High Sierra

### Donwload High Sierra
Download High Sierra from the App Store, if the download is not full, see kgp thread for a workaround to get it.

### Bootable USB
Erase USB Drive in the Disk Utility:
> Name: `USB`
> Format: `HFS+ [(Mac OS Extended (Journaled)]`
> Scheme: `GUID Partition Map`

Copy High Sierra and make it bootable:
```sh
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ macOS\ High\ Sierra.app --nointeraction

```
```sh
cd /Volumes/Install\ macOS\ High\ Sierra
mkdir .IABootFiles
cd .IABootFiles
cp /Volumes/Install\ macOS\ High\ Sierra/System/Library/CoreServices/boot.efi .

```

Mount the EFI partition of the USB Stick:
```sh
# List the volumes & disks
diskutil list
# Spot the IDENTIFIER of the EFI partition of the USB Stick from the list.

# Make the folder where we will mount the partition
sudo mkdir /Volumes/EFI

# Mount the EFI partition
sudo mount -t msdos /dev/IDENTIFIER /Volumes/EFI

```
Copy the `EFI` folder to the `/Volumes/EFI` mounted partition

### Prepare the target hard disk
Erase the SSD Drive:
> Name: `macOS`
> Format: `HFS+ [(Mac OS Extended (Journaled)]`
> Scheme: `GUID Partition Map`

Mount the EFI Partition (see above) & copy the `EFI` folder inside

### Installation
Boot into `USB EFI > USB Installer`. (You might need these boot arg `-v darkwake=0 keepsyms=1 debug=0x100`)
Install macOS to the SSD.
At the reboot, `macOS EFI > macOS`.
At the reboot, again, `macOS EFI > macOS`.
Finish macOS Install.
Start App Store and update to latest releases.

## Post-installation

### First boot
Update macOS through the App Store.

### 3rd party components
Install Creative Sound Blaster Play! 3 & NVIDIA Web Drivers.
Reboot & Adjust Resolution + Refresh Rate
**DO NOT CHANGE THE REFRESH RATE WITH G-SYNC ENABLED (Disable it, change it, enable it again)**

### macOS Post-install
Reboot, refer to `macOS_post-install` instructions, reboot then go back here.

### Clover
Using Clover Configurator, double check that everything set in the config.plist is correct (from the .diff seen earlier).
Make a backup and upgrade Clover if needed.

### CPU Cosmetic
```sh
cp /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/AppleSystemInfo.strings ~/Desktop/
sudo mv /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/AppleSystemInfo.strings /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/AppleSystemInfo.strings-Backup

```
Edit `AppleSystemInfo.strings` on the Desktop and change `UnknownCPUKind` to `4,6 GHz Intel Core i9-7980XE`
```sh
sudo codesign -f -s - ~/Desktop/AppleSystemInfo.strings

sudo cp ~/Desktop/AppleSystemInfo.strings /System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/English.lproj/

```

## Conclusion
Here we go, your setup should now be fully working.
You can now shutdown the PC, unplug the macOS disk, plug the (future) Windows disk, start the PC and install normally Windows.
Once it's done, shutdown the PC, plug back the macOS disk and be sure that it is the first disk in the boot order.
Now you can boot into macOS or Windows using Clover bootloader.
