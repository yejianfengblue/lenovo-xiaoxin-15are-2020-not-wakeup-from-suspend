# Lenovo Xiaoxin 15ARE2020-not-wakeup-from-suspend
Lenovo XiaoXin-15ARE 2020 (小新15 2020) AMD 4600U 16GB, Manjaro KDE, after sleep (suspend to RAM), can't wake up. The screen is black, the light of key ESC and the light next to power button are on. The solution is go to Windows and upgrade BIOS.

## System Info
```shell
inxi -Fxxxz                   
System:    Kernel: 5.9.10-1-MANJARO x86_64 bits: 64 compiler: gcc v: 10.2.0 Desktop: KDE Plasma 5.20.3 
           tk: Qt 5.15.1 wm: kwin_x11 dm: SDDM Distro: Manjaro Linux 
Machine:   Type: Laptop System: LENOVO product: 81YR v: Lenovo XiaoXin-15ARE 2020 serial: <filter> Chassis: 
           type: 10 v: Lenovo XiaoXin-15ARE 2020 serial: <filter> 
           Mobo: LENOVO model: LNVNB161216 v: SDK0L77769WIN serial: <filter> UEFI: LENOVO v: E7CN34WW 
           date: 10/12/2020 
Battery:   ID-1: BAT0 charge: 62.0 Wh condition: 64.5/70.0 Wh (92%) volts: 17.3/15.2 model: Celxpert L19C4PF1 
           type: Li-poly serial: <filter> status: Charging cycles: 9 
CPU:       Info: 6-Core model: AMD Ryzen 5 4600U with Radeon Graphics bits: 64 type: MT MCP arch: Zen rev: 1 
           L2 cache: 3072 KiB 
           flags: avx avx2 lm nx pae sse sse2 sse3 sse4_1 sse4_2 sse4a ssse3 svm bogomips: 50322 
           Speed: 1925 MHz min/max: 1400/2100 MHz boost: enabled Core speeds (MHz): 1: 1925 2: 1691 3: 1397 
           4: 1397 5: 1438 6: 1467 7: 1504 8: 1479 9: 1397 10: 1397 11: 1397 12: 1397 
Graphics:  Device-1: Advanced Micro Devices [AMD/ATI] Renoir vendor: Lenovo driver: amdgpu v: kernel 
           bus ID: 03:00.0 chip ID: 1002:1636 
           Device-2: Syntek Integrated Camera type: USB driver: uvcvideo bus ID: 1-3:2 chip ID: 174f:244c 
           serial: <filter> 
           Display: x11 server: X.Org 1.20.9 compositor: kwin_x11 driver: amdgpu,ati unloaded: modesetting 
           alternate: fbdev,vesa resolution: 1920x1080~60Hz s-dpi: 96 
           OpenGL: renderer: AMD RENOIR (DRM 3.39.0 5.9.10-1-MANJARO LLVM 11.0.0) v: 4.6 Mesa 20.2.2 
           direct render: Yes 
Audio:     Device-1: Advanced Micro Devices [AMD/ATI] vendor: Lenovo driver: snd_hda_intel v: kernel 
           bus ID: 03:00.1 chip ID: 1002:1637 
           Device-2: Advanced Micro Devices [AMD] Raven/Raven2/FireFlight/Renoir Audio Processor vendor: Lenovo 
           driver: N/A bus ID: 03:00.5 chip ID: 1022:15e2 
           Device-3: Advanced Micro Devices [AMD] Family 17h HD Audio vendor: Lenovo driver: snd_hda_intel 
           v: kernel bus ID: 03:00.6 chip ID: 1022:15e3 
           Sound Server: ALSA v: k5.9.10-1-MANJARO 
Network:   Device-1: Realtek RTL8822CE 802.11ac PCIe Wireless Network Adapter vendor: Lenovo driver: rtw_8822ce 
           v: N/A port: 2000 bus ID: 01:00.0 chip ID: 10ec:c822 
           IF: wlp1s0 state: up mac: <filter> 
           IF-ID-1: br-21baa5142257 state: down mac: <filter> 
           IF-ID-2: docker0 state: down mac: <filter> 
Drives:    Local Storage: total: 476.94 GiB used: 37.81 GiB (7.9%) 
           ID-1: /dev/nvme0n1 vendor: SK Hynix model: HFS512GD9TNI-L2A0B size: 476.94 GiB speed: 31.6 Gb/s 
           lanes: 4 serial: <filter> rev: 11010C10 scheme: GPT 
Partition: ID-1: / size: 107.77 GiB used: 27.14 GiB (25.2%) fs: ext4 dev: /dev/nvme0n1p6 
           ID-2: /home size: 146.65 GiB used: 10.64 GiB (7.3%) fs: ext4 dev: /dev/nvme0n1p5 
Swap:      ID-1: swap-1 type: partition size: 14.69 GiB used: 0 KiB (0.0%) priority: -2 dev: /dev/nvme0n1p7 
Sensors:   System Temperatures: cpu: 64.0 C mobo: N/A gpu: amdgpu temp: 49.0 C 
           Fan Speeds (RPM): N/A 
Info:      Processes: 326 Uptime: 1h 09m Memory: 15.07 GiB used: 2.80 GiB (18.6%) Init: systemd v: 246 
           Compilers: gcc: 10.2.0 Packages: pacman: 1374 Shell: Zsh v: 5.8 running in: konsole inxi: 3.1.08
```

## Bad Case
### Log

Error after line "kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0"

```shell
# Find start time
$ journalctl --system | grep "suspend entry"
Nov 27 14:18:57 x15 kernel: PM: suspend entry (deep)
Nov 27 14:19:00 x15 kernel: PM: suspend entry (s2idle)

# Find end time
$ journalctl --system | grep "suspend exit"
Nov 27 14:19:00 x15 kernel: PM: suspend exit
Nov 27 14:19:24 x15 kernel: PM: suspend exit

# Get log
$ journalctl --system --since "2020-11-27 14:18:56" --until "2020-11-27 14:19:25" 
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.5886] manager: sleep: sleep requested (sleeping: no  enabled: yes)
Nov 27 14:18:56 x15 ModemManager[700]: <info>  [sleep-monitor] system is about to suspend
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.5888] manager: NetworkManager state is now ASLEEP
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.5898] device (wlp1s0): state change: activated -> deactivating (reason 'sleeping', sys-iface-state: 'managed')
Nov 27 14:18:56 x15 dbus-daemon[568]: [system] Activating via systemd: service name='org.freedesktop.nm_dispatcher' unit='dbus-org.freedesktop.nm-dispatcher.service' requested by ':1.4' (uid=0 pid=570 comm="/usr/bin/NetworkManager --no-daemon " label="unconfined")
Nov 27 14:18:56 x15 polkitd[574]: Unregistered Authentication Agent for unix-process:3357:8443 (system bus name :1.104, object path /org/freedesktop/PolicyKit1/AuthenticationAgent, locale en_US.UTF-8) (disconnected from bus)
Nov 27 14:18:56 x15 systemd[1]: Starting Network Manager Script Dispatcher Service...
Nov 27 14:18:56 x15 dbus-daemon[568]: [system] Successfully activated service 'org.freedesktop.nm_dispatcher'
Nov 27 14:18:56 x15 systemd[1]: Started Network Manager Script Dispatcher Service.
Nov 27 14:18:56 x15 audit[1]: SERVICE_START pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=NetworkManager-dispatcher comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
Nov 27 14:18:56 x15 kernel: audit: type=1130 audit(1606457936.616:132): pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=NetworkManager-dispatcher comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
Nov 27 14:18:56 x15 kernel: wlp1s0: deauthenticating from d2:57:9b:84:31:04 by local choice (Reason: 3=DEAUTH_LEAVING)
Nov 27 14:18:56 x15 wpa_supplicant[960]: wlp1s0: CTRL-EVENT-DISCONNECTED bssid=d2:57:9b:84:31:04 reason=3 locally_generated=1
Nov 27 14:18:56 x15 wpa_supplicant[960]: wlp1s0: CTRL-EVENT-REGDOM-CHANGE init=CORE type=WORLD
Nov 27 14:18:56 x15 kernel: rtw_8822ce 0000:01:00.0: sta d2:57:9b:84:31:04 with macid 0 left
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.6748] device (wlp1s0): supplicant interface state: completed -> disconnected
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.6751] device (wlp1s0): state change: deactivating -> disconnected (reason 'sleeping', sys-iface-state: 'managed')
Nov 27 14:18:56 x15 avahi-daemon[566]: Withdrawing address record for 240e:47d:228:3b4c:79ab:218f:6c69:db1d on wlp1s0.
Nov 27 14:18:56 x15 avahi-daemon[566]: Leaving mDNS multicast group on interface wlp1s0.IPv6 with address 240e:47d:228:3b4c:79ab:218f:6c69:db1d.
Nov 27 14:18:56 x15 avahi-daemon[566]: Joining mDNS multicast group on interface wlp1s0.IPv6 with address fe80::b843:f35d:4d49:773b.
Nov 27 14:18:56 x15 avahi-daemon[566]: Registering new address record for fe80::b843:f35d:4d49:773b on wlp1s0.*.
Nov 27 14:18:56 x15 avahi-daemon[566]: Withdrawing address record for fe80::b843:f35d:4d49:773b on wlp1s0.
Nov 27 14:18:56 x15 avahi-daemon[566]: Leaving mDNS multicast group on interface wlp1s0.IPv6 with address fe80::b843:f35d:4d49:773b.
Nov 27 14:18:56 x15 avahi-daemon[566]: Interface wlp1s0.IPv6 no longer relevant for mDNS.
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.7104] dhcp4 (wlp1s0): canceled DHCP transaction
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.7106] dhcp4 (wlp1s0): state changed bound -> done
Nov 27 14:18:56 x15 avahi-daemon[566]: Interface wlp1s0.IPv4 no longer relevant for mDNS.
Nov 27 14:18:56 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0
Nov 27 14:18:56 x15 avahi-daemon[566]: Leaving mDNS multicast group on interface wlp1s0.IPv4 with address 192.168.43.179.
Nov 27 14:18:56 x15 avahi-daemon[566]: Withdrawing address record for 192.168.43.179 on wlp1s0.
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.7180] device (wlp1s0): set-hw-addr: set MAC address to D6:EB:79:B6:FC:A8 (scanning)
Nov 27 14:18:56 x15 avahi-daemon[566]: Joining mDNS multicast group on interface wlp1s0.IPv4 with address 192.168.43.179.
Nov 27 14:18:56 x15 avahi-daemon[566]: New relevant interface wlp1s0.IPv4 for mDNS.
Nov 27 14:18:56 x15 avahi-daemon[566]: Registering new address record for 192.168.43.179 on wlp1s0.IPv4.
Nov 27 14:18:56 x15 kernel: rtw_8822ce 0000:01:00.0: start vif d6:eb:79:b6:fc:a8 on port 0
Nov 27 14:18:56 x15 avahi-daemon[566]: Withdrawing address record for 192.168.43.179 on wlp1s0.
Nov 27 14:18:56 x15 avahi-daemon[566]: Leaving mDNS multicast group on interface wlp1s0.IPv4 with address 192.168.43.179.
Nov 27 14:18:56 x15 avahi-daemon[566]: Interface wlp1s0.IPv4 no longer relevant for mDNS.
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.9565] device (wlp1s0): supplicant interface state: disconnected -> interface_disabled
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.9568] device (wlp1s0): state change: disconnected -> unmanaged (reason 'sleeping', sys-iface-state: 'managed')
Nov 27 14:18:56 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif d6:eb:79:b6:fc:a8 on port 0
Nov 27 14:18:56 x15 NetworkManager[570]: <info>  [1606457936.9627] device (wlp1s0): set-hw-addr: reset MAC address to 4C:EB:BD:95:27:03 (unmanage)
Nov 27 14:18:57 x15 kernel: rtw_8822ce 0000:01:00.0: start vif 4c:eb:bd:95:27:03 on port 0
Nov 27 14:18:57 x15 systemd[1]: Reached target Sleep.
Nov 27 14:18:57 x15 systemd[1]: Starting Suspend...
Nov 27 14:18:57 x15 wpa_supplicant[960]: nl80211: deinit ifname=wlp1s0 disabled_11b_rates=0
Nov 27 14:18:57 x15 systemd-sleep[3403]: Suspending system...
Nov 27 14:18:57 x15 kernel: PM: suspend entry (deep)
Nov 27 14:18:59 x15 kernel: Filesystems sync: 0.011 seconds
Nov 27 14:18:59 x15 kernel: Freezing user space processes ... (elapsed 0.003 seconds) done.
Nov 27 14:18:59 x15 kernel: OOM killer disabled.
Nov 27 14:18:59 x15 kernel: Freezing remaining freezable tasks ... (elapsed 0.001 seconds) done.
Nov 27 14:18:59 x15 kernel: printk: Suspending console(s) (use no_console_suspend to debug)
Nov 27 14:18:59 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0
Nov 27 14:18:59 x15 kernel: xhci_hcd 0000:03:00.4: WARN: xHC save state timeout
Nov 27 14:18:59 x15 kernel: PM: suspend_common(): xhci_pci_suspend+0x0/0x140 [xhci_pci] returns -110
Nov 27 14:18:59 x15 kernel: PM: pci_pm_suspend(): hcd_pci_suspend+0x0/0x30 returns -110
Nov 27 14:18:59 x15 kernel: PM: dpm_run_callback(): pci_pm_suspend+0x0/0x160 returns -110
Nov 27 14:18:59 x15 kernel: PM: Device 0000:03:00.4 failed to suspend async: error -110
Nov 27 14:18:59 x15 kernel: [drm] free PSP TMR buffer
Nov 27 14:18:59 x15 kernel: ata2: failed stop FIS RX (-16)
Nov 27 14:18:59 x15 kernel: ata2: SATA link down (SStatus 0 SControl 300)
Nov 27 14:18:59 x15 kernel: PM: Some devices failed to suspend, or early wake event detected
Nov 27 14:18:59 x15 kernel: ------------[ cut here ]------------
Nov 27 14:18:59 x15 kernel: WARNING: CPU: 11 PID: 178 at drivers/ata/libata-eh.c:3950 ata_scsi_port_error_handler+0x84c/0x880
Nov 27 14:18:59 x15 kernel: Modules linked in: xt_conntrack xt_MASQUERADE nf_conntrack_netlink nfnetlink xfrm_user xfrm_algo xt_addrtype iptable_filter iptable_nat nf_nat nf_conntrack nf_defrag_ipv6 nf_defrag_ipv4 libcrc32c br_netfilter bridge stp llc ccm snd_seq_dummy snd_hrtimer snd_seq snd_seq_device fuse uvcvideo bnep videobuf2_vmalloc videobuf2_memops videobuf2_v4l2 videobuf2_common btusb btrtl btbcm btintel bluetooth videodev ecdh_generic mc ecc squashfs joydev mousedev loop hid_multitouch overlay hid_generic wmi_bmof amdgpu rtw88_8822ce rtw88_8822c edac_mce_amd rtw88_pci kvm_amd rtw88_core snd_hda_codec_realtek snd_hda_codec_generic mac80211 kvm ledtrig_audio snd_hda_codec_hdmi gpu_sched irqbypass snd_hda_intel i2c_algo_bit crct10dif_pclmul snd_intel_dspcfg crc32_pclmul ttm ghash_clmulni_intel nls_iso8859_1 aesni_intel snd_hda_codec nls_cp437 vfat crypto_simd fat cfg80211 cryptd drm_kms_helper snd_hda_core glue_helper rapl snd_hwdep snd_pcm cec input_leds pcspkr snd_timer rc_core ucsi_acpi
Nov 27 14:18:59 x15 kernel:  ideapad_laptop syscopyarea snd sp5100_tco sparse_keymap sysfillrect sysimgblt snd_rn_pci_acp3x libarc4 ccp snd_pci_acp3x fb_sys_fops soundcore i2c_piix4 k10temp rfkill typec_ucsi typec wmi tpm_crb battery i2c_hid tpm_tis hid tpm_tis_core tpm evdev acpi_cpufreq pinctrl_amd mac_hid ac rng_core uinput drm msr sg crypto_user agpgart ip_tables x_tables ext4 crc32c_generic crc16 mbcache jbd2 serio_raw atkbd libps2 crc32c_intel xhci_pci xhci_hcd i8042 serio
Nov 27 14:18:59 x15 kernel: CPU: 11 PID: 178 Comm: scsi_eh_1 Tainted: G        W         5.9.10-1-MANJARO #1
Nov 27 14:18:59 x15 kernel: Hardware name: LENOVO 81YR/LNVNB161216, BIOS E7CN24WW 03/25/2020
Nov 27 14:18:59 x15 kernel: RIP: 0010:ata_scsi_port_error_handler+0x84c/0x880
Nov 27 14:18:59 x15 kernel: Code: 08 de 2d 00 8b 43 20 e9 44 fe ff ff e8 fd 8e 00 00 48 8b 7b 10 e8 94 76 30 00 49 89 c5 8b 43 20 25 ff ff fb ff e9 ca fc ff ff <0f> 0b e9 53 f8 ff ff 0f 0b e9 a1 fb ff ff 0f 0b e9 c8 fd ff ff 0f
Nov 27 14:18:59 x15 kernel: hub 3-0:1.0: hub_ext_port_status failed (err = -108)
Nov 27 14:18:59 x15 kernel: RSP: 0018:ffffa840c059fe28 EFLAGS: 00010246
Nov 27 14:18:59 x15 kernel: RAX: 0000000080000000 RBX: ffff9f6db99cc000 RCX: 0000000000000000
Nov 27 14:18:59 x15 kernel: usb usb3-port3: cannot disable (err = -108)
Nov 27 14:18:59 x15 kernel: RDX: 0000000000000001 RSI: 0000000000000282 RDI: 00000000ffffffff
Nov 27 14:18:59 x15 kernel: RBP: 00000000000002aa R08: 0000000000000000 R09: 0000000000000000
Nov 27 14:18:59 x15 kernel: R10: 0000000000001375 R11: 0000000000000000 R12: ffff9f6db99cc000
Nov 27 14:18:59 x15 kernel: R13: ffff9f6dbd192080 R14: ffff9f6dbd192208 R15: ffff9f6dbd192000
Nov 27 14:18:59 x15 kernel: FS:  0000000000000000(0000) GS:ffff9f6dbf6c0000(0000) knlGS:0000000000000000
Nov 27 14:18:59 x15 kernel: hub 3-0:1.0: hub_ext_port_status failed (err = -108)
Nov 27 14:18:59 x15 kernel: CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
Nov 27 14:18:59 x15 kernel: CR2: 00007f863c006176 CR3: 0000000012e0e000 CR4: 0000000000350ee0
Nov 27 14:18:59 x15 kernel: usb usb3-port4: cannot disable (err = -108)
Nov 27 14:18:59 x15 kernel: Call Trace:
Nov 27 14:18:59 x15 kernel:  ata_scsi_error+0x91/0xc0
Nov 27 14:18:59 x15 kernel:  scsi_error_handler+0xd1/0x510
Nov 27 14:18:59 x15 kernel:  ? preempt_count_add+0x68/0xa0
Nov 27 14:18:59 x15 kernel:  ? scsi_eh_get_sense+0x1f0/0x1f0
Nov 27 14:18:59 x15 kernel:  kthread+0x142/0x160
Nov 27 14:18:59 x15 kernel:  ? __kthread_bind_mask+0x60/0x60
Nov 27 14:18:59 x15 kernel:  ret_from_fork+0x22/0x30
Nov 27 14:18:59 x15 kernel: ---[ end trace 9f156411e2961389 ]---
Nov 27 14:18:59 x15 kernel: [drm] PCIE GART of 1024M enabled (table at 0x000000F400900000).
Nov 27 14:18:59 x15 kernel: [drm] PSP is resuming...
Nov 27 14:18:59 x15 kernel: [drm] reserve 0x400000 from 0xf41f800000 for PSP TMR
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: SMU is resuming...
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: dpm has been disabled
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: SMU is resumed successfully!
Nov 27 14:18:59 x15 kernel: rtw_8822ce 0000:01:00.0: start vif 4c:eb:bd:95:27:03 on port 0
Nov 27 14:18:59 x15 kernel: nvme nvme0: 16/0/0 default/read/poll queues
Nov 27 14:18:59 x15 kernel: ata1: SATA link down (SStatus 0 SControl 300)
Nov 27 14:18:59 x15 kernel: ata2: SATA link down (SStatus 0 SControl 300)
Nov 27 14:18:59 x15 kernel: [drm] kiq ring mec 2 pipe 1 q 0
Nov 27 14:18:59 x15 kernel: [drm] DMUB hardware initialized: version=0x01000000
Nov 27 14:18:59 x15 kernel: [drm] Failed to add display topology, DTM TA is not initialized.
Nov 27 14:18:59 x15 kernel: [drm] VCN decode and encode initialized successfully(under DPG Mode).
Nov 27 14:18:59 x15 kernel: [drm] JPEG decode initialized successfully.
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring gfx uses VM inv eng 0 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.0.0 uses VM inv eng 1 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.1.0 uses VM inv eng 4 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.2.0 uses VM inv eng 5 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.3.0 uses VM inv eng 6 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.0.1 uses VM inv eng 7 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.1.1 uses VM inv eng 8 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.2.1 uses VM inv eng 9 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.3.1 uses VM inv eng 10 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring kiq_2.1.0 uses VM inv eng 11 on hub 0
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring sdma0 uses VM inv eng 0 on hub 1
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_dec uses VM inv eng 1 on hub 1
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_enc0 uses VM inv eng 4 on hub 1
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_enc1 uses VM inv eng 5 on hub 1
Nov 27 14:18:59 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring jpeg_dec uses VM inv eng 6 on hub 1
Nov 27 14:18:59 x15 kernel: acpi LNXPOWER:04: Turning OFF
Nov 27 14:18:59 x15 kernel: acpi LNXPOWER:02: Turning OFF
Nov 27 14:18:59 x15 kernel: acpi LNXPOWER:00: Turning OFF
Nov 27 14:18:59 x15 kernel: OOM killer enabled.
Nov 27 14:18:59 x15 kernel: Restarting tasks ... done.
Nov 27 14:19:00 x15 kernel: PM: suspend exit
Nov 27 14:19:00 x15 kernel: PM: suspend entry (s2idle)
Nov 27 14:19:24 x15 kernel: Filesystems sync: 0.013 seconds
Nov 27 14:19:24 x15 kernel: Freezing user space processes ... (elapsed 0.009 seconds) done.
Nov 27 14:19:24 x15 kernel: OOM killer disabled.
Nov 27 14:19:24 x15 kernel: Freezing remaining freezable tasks ... (elapsed 0.001 seconds) done.
Nov 27 14:19:24 x15 kernel: printk: Suspending console(s) (use no_console_suspend to debug)
Nov 27 14:19:24 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0
Nov 27 14:19:24 x15 kernel: [drm] Register(0) [mmUVD_POWER_STATUS] failed to reach value 0x00000001 != 0x00000002
Nov 27 14:19:24 x15 kernel: [drm] Register(0) [mmUVD_POWER_STATUS] failed to reach value 0x00000001 != 0x00000002
Nov 27 14:19:24 x15 kernel: [drm] free PSP TMR buffer
Nov 27 14:19:24 x15 kernel: ata2: failed stop FIS RX (-16)
Nov 27 14:19:24 x15 kernel: ata2: SATA link down (SStatus 0 SControl 300)
Nov 27 14:19:24 x15 kernel: ACPI: EC: interrupt blocked
Nov 27 14:19:24 x15 kernel: ACPI: EC: interrupt unblocked
Nov 27 14:19:24 x15 kernel: ------------[ cut here ]------------
Nov 27 14:19:24 x15 kernel: WARNING: CPU: 1 PID: 3403 at kernel/irq/chip.c:210 irq_startup+0xe1/0xf0
Nov 27 14:19:24 x15 kernel: Modules linked in: xt_conntrack xt_MASQUERADE nf_conntrack_netlink nfnetlink xfrm_user xfrm_algo xt_addrtype iptable_filter iptable_nat nf_nat nf_conntrack nf_defrag_ipv6 nf_defrag_ipv4 libcrc32c br_netfilter bridge stp llc ccm snd_seq_dummy snd_hrtimer snd_seq snd_seq_device fuse uvcvideo bnep videobuf2_vmalloc videobuf2_memops videobuf2_v4l2 videobuf2_common btusb btrtl btbcm btintel bluetooth videodev ecdh_generic mc ecc squashfs joydev mousedev loop hid_multitouch overlay hid_generic wmi_bmof amdgpu rtw88_8822ce rtw88_8822c edac_mce_amd rtw88_pci kvm_amd rtw88_core snd_hda_codec_realtek snd_hda_codec_generic mac80211 kvm ledtrig_audio snd_hda_codec_hdmi gpu_sched irqbypass snd_hda_intel i2c_algo_bit crct10dif_pclmul snd_intel_dspcfg crc32_pclmul ttm ghash_clmulni_intel nls_iso8859_1 aesni_intel snd_hda_codec nls_cp437 vfat crypto_simd fat cfg80211 cryptd drm_kms_helper snd_hda_core glue_helper rapl snd_hwdep snd_pcm cec input_leds pcspkr snd_timer rc_core ucsi_acpi
Nov 27 14:19:24 x15 kernel:  ideapad_laptop syscopyarea snd sp5100_tco sparse_keymap sysfillrect sysimgblt snd_rn_pci_acp3x libarc4 ccp snd_pci_acp3x fb_sys_fops soundcore i2c_piix4 k10temp rfkill typec_ucsi typec wmi tpm_crb battery i2c_hid tpm_tis hid tpm_tis_core tpm evdev acpi_cpufreq pinctrl_amd mac_hid ac rng_core uinput drm msr sg crypto_user agpgart ip_tables x_tables ext4 crc32c_generic crc16 mbcache jbd2 serio_raw atkbd libps2 crc32c_intel xhci_pci xhci_hcd i8042 serio
Nov 27 14:19:24 x15 kernel: CPU: 1 PID: 3403 Comm: systemd-sleep Tainted: G        W         5.9.10-1-MANJARO #1
Nov 27 14:19:24 x15 kernel: Hardware name: LENOVO 81YR/LNVNB161216, BIOS E7CN24WW 03/25/2020
Nov 27 14:19:24 x15 kernel: RIP: 0010:irq_startup+0xe1/0xf0
Nov 27 14:19:24 x15 kernel: Code: f6 4c 89 e7 e8 e0 41 00 00 85 c0 75 21 4c 89 e7 31 d2 4c 89 ee e8 cf c3 ff ff 48 89 ef e8 b7 fe ff ff 41 89 c4 e9 51 ff ff ff <0f> 0b eb b6 0f 0b eb b2 0f 1f 80 00 00 00 00 0f 1f 44 00 00 55 48
Nov 27 14:19:24 x15 kernel: RSP: 0018:ffffa840c9667da0 EFLAGS: 00010002
Nov 27 14:19:24 x15 kernel: RAX: 0000000000000140 RBX: 0000000000000001 RCX: 0000000000000140
Nov 27 14:19:24 x15 kernel: RDX: 0000000000000004 RSI: ffffffffaad756a0 RDI: ffff9f6cb3ecb418
Nov 27 14:19:24 x15 kernel: RBP: ffff9f6cb3ecb400 R08: 0000000000000000 R09: 0000000000000140
Nov 27 14:19:24 x15 kernel: R10: 0000000000000000 R11: 0000000000000000 R12: 0000000000000001
Nov 27 14:19:24 x15 kernel: R13: ffff9f6cb3ecb418 R14: ffff9f6cb3ecb4e4 R15: 0000000000000000
Nov 27 14:19:24 x15 kernel: FS:  00007fe95d9cf200(0000) GS:ffff9f6dbf440000(0000) knlGS:0000000000000000
Nov 27 14:19:24 x15 kernel: CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
Nov 27 14:19:24 x15 kernel: CR2: 000055f72efc6a60 CR3: 00000002c4bf2000 CR4: 0000000000350ee0
Nov 27 14:19:24 x15 kernel: Call Trace:
Nov 27 14:19:24 x15 kernel:  resume_irqs+0xb6/0xf0
Nov 27 14:19:24 x15 kernel:  dpm_resume_noirq+0xf/0x20
Nov 27 14:19:24 x15 kernel:  suspend_devices_and_enter+0x333/0x8a0
Nov 27 14:19:24 x15 kernel:  pm_suspend.cold+0x329/0x374
Nov 27 14:19:24 x15 kernel:  state_store+0x42/0x90
Nov 27 14:19:24 x15 kernel:  kernfs_fop_write+0xce/0x1b0
Nov 27 14:19:24 x15 kernel:  vfs_write+0xc7/0x210
Nov 27 14:19:24 x15 kernel:  ksys_write+0x67/0xe0
Nov 27 14:19:24 x15 kernel:  do_syscall_64+0x33/0x40
Nov 27 14:19:24 x15 kernel:  entry_SYSCALL_64_after_hwframe+0x44/0xa9
Nov 27 14:19:24 x15 kernel: RIP: 0033:0x7fe95e7eaf67
Nov 27 14:19:24 x15 kernel: Code: 0d 00 f7 d8 64 89 02 48 c7 c0 ff ff ff ff eb b7 0f 1f 00 f3 0f 1e fa 64 8b 04 25 18 00 00 00 85 c0 75 10 b8 01 00 00 00 0f 05 <48> 3d 00 f0 ff ff 77 51 c3 48 83 ec 28 48 89 54 24 18 48 89 74 24
Nov 27 14:19:24 x15 kernel: RSP: 002b:00007ffd5b9bff18 EFLAGS: 00000246 ORIG_RAX: 0000000000000001
Nov 27 14:19:24 x15 kernel: RAX: ffffffffffffffda RBX: 0000000000000007 RCX: 00007fe95e7eaf67
Nov 27 14:19:24 x15 kernel: RDX: 0000000000000007 RSI: 0000564ed8270a90 RDI: 0000000000000004
Nov 27 14:19:24 x15 kernel: RBP: 0000564ed8270a90 R08: 0000000000000001 R09: 00007fe95e8bca60
Nov 27 14:19:24 x15 kernel: R10: 0000000000000070 R11: 0000000000000246 R12: 0000000000000007
Nov 27 14:19:24 x15 kernel: R13: 0000564ed826b3c0 R14: 0000000000000007 R15: 00007fe95e8bd720
Nov 27 14:19:24 x15 kernel: ---[ end trace 9f156411e296138a ]---
Nov 27 14:19:24 x15 kernel: [drm] PCIE GART of 1024M enabled (table at 0x000000F400900000).
Nov 27 14:19:24 x15 kernel: [drm] PSP is resuming...
Nov 27 14:19:24 x15 kernel: [drm] reserve 0x400000 from 0xf41f800000 for PSP TMR
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: SMU is resuming...
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: dpm has been disabled
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: SMU is resumed successfully!
Nov 27 14:19:24 x15 kernel: rtw_8822ce 0000:01:00.0: start vif 4c:eb:bd:95:27:03 on port 0
Nov 27 14:19:24 x15 kernel: ata1: SATA link down (SStatus 0 SControl 300)
Nov 27 14:19:24 x15 kernel: [drm] kiq ring mec 2 pipe 1 q 0
Nov 27 14:19:24 x15 kernel: [drm] DMUB hardware initialized: version=0x01000000
Nov 27 14:19:24 x15 kernel: [drm] Failed to add display topology, DTM TA is not initialized.
Nov 27 14:19:24 x15 kernel: [drm] VCN decode and encode initialized successfully(under DPG Mode).
Nov 27 14:19:24 x15 kernel: [drm] JPEG decode initialized successfully.
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring gfx uses VM inv eng 0 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.0.0 uses VM inv eng 1 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.1.0 uses VM inv eng 4 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.2.0 uses VM inv eng 5 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.3.0 uses VM inv eng 6 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.0.1 uses VM inv eng 7 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.1.1 uses VM inv eng 8 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.2.1 uses VM inv eng 9 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.3.1 uses VM inv eng 10 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring kiq_2.1.0 uses VM inv eng 11 on hub 0
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring sdma0 uses VM inv eng 0 on hub 1
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_dec uses VM inv eng 1 on hub 1
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_enc0 uses VM inv eng 4 on hub 1
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_enc1 uses VM inv eng 5 on hub 1
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring jpeg_dec uses VM inv eng 6 on hub 1
Nov 27 14:19:24 x15 kernel: ahci 0000:04:00.1: failed stop FIS RX (-16)
Nov 27 14:19:24 x15 kernel: ------------[ cut here ]------------
Nov 27 14:19:24 x15 kernel: WARNING: CPU: 10 PID: 178 at drivers/ata/libata-eh.c:3950 ata_scsi_port_error_handler+0x84c/0x880
Nov 27 14:19:24 x15 kernel: Modules linked in: xt_conntrack xt_MASQUERADE nf_conntrack_netlink nfnetlink xfrm_user xfrm_algo xt_addrtype iptable_filter iptable_nat nf_nat nf_conntrack nf_defrag_ipv6 nf_defrag_ipv4 libcrc32c br_netfilter bridge stp llc ccm snd_seq_dummy snd_hrtimer snd_seq snd_seq_device fuse uvcvideo bnep videobuf2_vmalloc videobuf2_memops videobuf2_v4l2 videobuf2_common btusb btrtl btbcm btintel bluetooth videodev ecdh_generic mc ecc squashfs joydev mousedev loop hid_multitouch overlay hid_generic wmi_bmof amdgpu rtw88_8822ce rtw88_8822c edac_mce_amd rtw88_pci kvm_amd rtw88_core snd_hda_codec_realtek snd_hda_codec_generic mac80211 kvm ledtrig_audio snd_hda_codec_hdmi gpu_sched irqbypass snd_hda_intel i2c_algo_bit crct10dif_pclmul snd_intel_dspcfg crc32_pclmul ttm ghash_clmulni_intel nls_iso8859_1 aesni_intel snd_hda_codec nls_cp437 vfat crypto_simd fat cfg80211 cryptd drm_kms_helper snd_hda_core glue_helper rapl snd_hwdep snd_pcm cec input_leds pcspkr snd_timer rc_core ucsi_acpi
Nov 27 14:19:24 x15 kernel:  ideapad_laptop syscopyarea snd sp5100_tco sparse_keymap sysfillrect sysimgblt snd_rn_pci_acp3x libarc4 ccp snd_pci_acp3x fb_sys_fops soundcore i2c_piix4 k10temp rfkill typec_ucsi typec wmi tpm_crb battery i2c_hid tpm_tis hid tpm_tis_core tpm evdev acpi_cpufreq pinctrl_amd mac_hid ac rng_core uinput drm msr sg crypto_user agpgart ip_tables x_tables ext4 crc32c_generic crc16 mbcache jbd2 serio_raw atkbd libps2 crc32c_intel xhci_pci xhci_hcd i8042 serio
Nov 27 14:19:24 x15 kernel: CPU: 10 PID: 178 Comm: scsi_eh_1 Tainted: G        W         5.9.10-1-MANJARO #1
Nov 27 14:19:24 x15 kernel: Hardware name: LENOVO 81YR/LNVNB161216, BIOS E7CN24WW 03/25/2020
Nov 27 14:19:24 x15 kernel: RIP: 0010:ata_scsi_port_error_handler+0x84c/0x880
Nov 27 14:19:24 x15 kernel: Code: 08 de 2d 00 8b 43 20 e9 44 fe ff ff e8 fd 8e 00 00 48 8b 7b 10 e8 94 76 30 00 49 89 c5 8b 43 20 25 ff ff fb ff e9 ca fc ff ff <0f> 0b e9 53 f8 ff ff 0f 0b e9 a1 fb ff ff 0f 0b e9 c8 fd ff ff 0f
Nov 27 14:19:24 x15 kernel: RSP: 0018:ffffa840c059fe28 EFLAGS: 00010246
Nov 27 14:19:24 x15 kernel: RAX: 0000000080000000 RBX: ffff9f6db99cc000 RCX: 0000000000000000
Nov 27 14:19:24 x15 kernel: RDX: 0000000000000001 RSI: 0000000000000282 RDI: 00000000ffffffff
Nov 27 14:19:24 x15 kernel: RBP: 00000000000002aa R08: 0000000000000000 R09: 0000000000000000
Nov 27 14:19:24 x15 kernel: R10: 0000000000000211 R11: 0000000000000000 R12: ffff9f6db99cc000
Nov 27 14:19:24 x15 kernel: R13: ffff9f6dbd192080 R14: ffff9f6dbd192208 R15: ffff9f6dbd192000
Nov 27 14:19:24 x15 kernel: FS:  0000000000000000(0000) GS:ffff9f6dbf680000(0000) knlGS:0000000000000000
Nov 27 14:19:24 x15 kernel: CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
Nov 27 14:19:24 x15 kernel: CR2: 00007ffba4372000 CR3: 0000000012e0e000 CR4: 0000000000350ee0
Nov 27 14:19:24 x15 kernel: Call Trace:
Nov 27 14:19:24 x15 kernel:  ata_scsi_error+0x91/0xc0
Nov 27 14:19:24 x15 kernel:  scsi_error_handler+0xd1/0x510
Nov 27 14:19:24 x15 kernel:  ? preempt_count_add+0x68/0xa0
Nov 27 14:19:24 x15 kernel:  ? scsi_eh_get_sense+0x1f0/0x1f0
Nov 27 14:19:24 x15 kernel:  kthread+0x142/0x160
Nov 27 14:19:24 x15 kernel:  ? __kthread_bind_mask+0x60/0x60
Nov 27 14:19:24 x15 kernel:  ret_from_fork+0x22/0x30
Nov 27 14:19:24 x15 kernel: ---[ end trace 9f156411e296138b ]---
Nov 27 14:19:24 x15 kernel: [drm] Fence fallback timer expired on ring sdma0
Nov 27 14:19:24 x15 kernel: ata2: SATA link down (SStatus 0 SControl 300)
Nov 27 14:19:24 x15 kernel: amdgpu 0000:03:00.0: [drm:amdgpu_ib_ring_tests [amdgpu]] *ERROR* IB test failed on gfx (-110).
Nov 27 14:19:24 x15 kernel: [drm:process_one_work] *ERROR* ib ring test failed (-110).
Nov 27 14:19:24 x15 kernel: OOM killer enabled.
Nov 27 14:19:24 x15 kernel: Restarting tasks ... 
Nov 27 14:19:24 x15 kernel: usb 3-3: USB disconnect, device number 2
Nov 27 14:19:24 x15 kernel: done.
Nov 27 14:19:24 x15 kernel: audit: type=1130 audit(1606457964.735:133): pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=systemd-rfkill comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
Nov 27 14:19:24 x15 audit[1]: SERVICE_START pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=systemd-rfkill comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
Nov 27 14:19:24 x15 systemd[1]: Starting Load/Save RF Kill Switch Status...
Nov 27 14:19:24 x15 systemd[1]: Stopped target Bluetooth.
Nov 27 14:19:24 x15 systemd[1]: Started Load/Save RF Kill Switch Status.
Nov 27 14:19:24 x15 systemd-sleep[3403]: System resumed.
Nov 27 14:19:24 x15 kernel: PM: suspend exit
```

### Diagnose
From below log which is extracted from above journalctl log, it is device `0000:03:00.4` causes suspend failure.
```log
Nov 27 14:18:59 x15 kernel: xhci_hcd 0000:03:00.4: WARN: xHC save state timeout
Nov 27 14:18:59 x15 kernel: PM: suspend_common(): xhci_pci_suspend+0x0/0x140 [xhci_pci] returns -110
Nov 27 14:18:59 x15 kernel: PM: pci_pm_suspend(): hcd_pci_suspend+0x0/0x30 returns -110
Nov 27 14:18:59 x15 kernel: PM: dpm_run_callback(): pci_pm_suspend+0x0/0x160 returns -110
Nov 27 14:18:59 x15 kernel: PM: Device 0000:03:00.4 failed to suspend async: error -110
Nov 27 14:18:59 x15 kernel: [drm] free PSP TMR buffer
Nov 27 14:18:59 x15 kernel: ata2: failed stop FIS RX (-16)
Nov 27 14:18:59 x15 kernel: ata2: SATA link down (SStatus 0 SControl 300)
Nov 27 14:18:59 x15 kernel: PM: Some devices failed to suspend, or early wake event detected
```

Check with command `lshw`, device `0000:03:00.4` is USB controller. But no external device like external keyboard or mouse is connected with USB.
```
*-usb:1
    description: USB controller
    product: Renoir USB 3.1
    vendor: Advanced Micro Devices, Inc. [AMD]
    physical id: 0.4
    bus info: pci@0000:03:00.4
    version: 00
    width: 64 bits
    clock: 33MHz
    capabilities: xhci bus_master cap_list
    configuration: driver=xhci_hcd latency=0
    resources: irq:61 memory:fd200000-fd2fffff
```

## Action Taken
1. Disable `Always On USB` in BIOS. No luck.
2. I have Windows + Manjaro duel boot. I went to Windows and upgraded BIOS to [version E7CN34WW 2020-11-02](https://newsupport.lenovo.com.cn/driveList.html?fromsource=driveList&selname=%E5%B0%8F%E6%96%B0-15%202020(AMD%E5%B9%B3%E5%8F%B0%EF%BC%9AARE%E7%89%88)). Lucky.

## Good Case

### Log

Sleep and resume without problem.

No error after line "kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0"

```shell
$ journalctl --system | grep "suspend entry"
Nov 27 19:33:02 x15 kernel: PM: suspend entry (deep)

$ journalctl --system | grep "suspend exit"
Nov 27 19:33:26 x15 kernel: PM: suspend exit

$ journalctl --system --since "2020-11-27 19:33:01" --until "2020-11-27 19:33:27" 
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6381] manager: sleep: sleep requested (sleeping: no  enabled: yes)
Nov 27 19:33:01 x15 ModemManager[553]: <info>  [sleep-monitor] system is about to suspend
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6383] manager: NetworkManager state is now ASLEEP
Nov 27 19:33:01 x15 polkitd[485]: Unregistered Authentication Agent for unix-process:89149:248852 (system bus name :1.152, object path /org/freedesktop/PolicyKit1/AuthenticationAgent, locale en_US.UTF-8) (disconnected from bus)
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6396] device (wlp1s0): state change: activated -> deactivating (reason 'sleeping', sys-iface-state: 'managed')
Nov 27 19:33:01 x15 dbus-daemon[482]: [system] Activating via systemd: service name='org.freedesktop.nm_dispatcher' unit='dbus-org.freedesktop.nm-dispatcher.service' requested by ':1.5' (uid=0 pid=483 comm="/usr/bin/NetworkManager --no>
Nov 27 19:33:01 x15 systemd[1]: Starting Network Manager Script Dispatcher Service...
Nov 27 19:33:01 x15 dbus-daemon[482]: [system] Successfully activated service 'org.freedesktop.nm_dispatcher'
Nov 27 19:33:01 x15 systemd[1]: Started Network Manager Script Dispatcher Service.
Nov 27 19:33:01 x15 audit[1]: SERVICE_START pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=NetworkManager-dispatcher comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
Nov 27 19:33:01 x15 kernel: audit: type=1130 audit(1606476781.647:139): pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=NetworkManager-dispatcher comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? ter>
Nov 27 19:33:01 x15 kernel: wlp1s0: deauthenticating from d2:57:9b:84:31:04 by local choice (Reason: 3=DEAUTH_LEAVING)
Nov 27 19:33:01 x15 wpa_supplicant[1299]: wlp1s0: CTRL-EVENT-DISCONNECTED bssid=d2:57:9b:84:31:04 reason=3 locally_generated=1
Nov 27 19:33:01 x15 wpa_supplicant[1299]: wlp1s0: CTRL-EVENT-REGDOM-CHANGE init=CORE type=WORLD
Nov 27 19:33:01 x15 kernel: rtw_8822ce 0000:01:00.0: sta d2:57:9b:84:31:04 with macid 0 left
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6902] device (wlp1s0): supplicant interface state: completed -> disconnected
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6906] device (wlp1s0): state change: deactivating -> disconnected (reason 'sleeping', sys-iface-state: 'managed')
Nov 27 19:33:01 x15 avahi-daemon[480]: Withdrawing address record for 240e:47d:228:3b4c:79ab:218f:6c69:db1d on wlp1s0.
Nov 27 19:33:01 x15 avahi-daemon[480]: Leaving mDNS multicast group on interface wlp1s0.IPv6 with address 240e:47d:228:3b4c:79ab:218f:6c69:db1d.
Nov 27 19:33:01 x15 avahi-daemon[480]: Joining mDNS multicast group on interface wlp1s0.IPv6 with address fe80::b843:f35d:4d49:773b.
Nov 27 19:33:01 x15 avahi-daemon[480]: Registering new address record for fe80::b843:f35d:4d49:773b on wlp1s0.*.
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6943] dhcp4 (wlp1s0): canceled DHCP transaction
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6944] dhcp4 (wlp1s0): state changed extended -> done
Nov 27 19:33:01 x15 avahi-daemon[480]: Withdrawing address record for fe80::b843:f35d:4d49:773b on wlp1s0.
Nov 27 19:33:01 x15 avahi-daemon[480]: Leaving mDNS multicast group on interface wlp1s0.IPv6 with address fe80::b843:f35d:4d49:773b.
Nov 27 19:33:01 x15 avahi-daemon[480]: Interface wlp1s0.IPv6 no longer relevant for mDNS.
Nov 27 19:33:01 x15 avahi-daemon[480]: Interface wlp1s0.IPv4 no longer relevant for mDNS.
Nov 27 19:33:01 x15 avahi-daemon[480]: Leaving mDNS multicast group on interface wlp1s0.IPv4 with address 192.168.43.179.
Nov 27 19:33:01 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0
Nov 27 19:33:01 x15 avahi-daemon[480]: Withdrawing address record for 192.168.43.179 on wlp1s0.
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.6995] device (wlp1s0): set-hw-addr: set MAC address to 6E:5E:68:B8:6C:FF (scanning)
Nov 27 19:33:01 x15 avahi-daemon[480]: Joining mDNS multicast group on interface wlp1s0.IPv4 with address 192.168.43.179.
Nov 27 19:33:01 x15 avahi-daemon[480]: New relevant interface wlp1s0.IPv4 for mDNS.
Nov 27 19:33:01 x15 avahi-daemon[480]: Registering new address record for 192.168.43.179 on wlp1s0.IPv4.
Nov 27 19:33:01 x15 avahi-daemon[480]: Withdrawing address record for 192.168.43.179 on wlp1s0.
Nov 27 19:33:01 x15 avahi-daemon[480]: Leaving mDNS multicast group on interface wlp1s0.IPv4 with address 192.168.43.179.
Nov 27 19:33:01 x15 kernel: rtw_8822ce 0000:01:00.0: start vif 6e:5e:68:b8:6c:ff on port 0
Nov 27 19:33:01 x15 avahi-daemon[480]: Interface wlp1s0.IPv4 no longer relevant for mDNS.
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.9327] device (wlp1s0): supplicant interface state: disconnected -> interface_disabled
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.9329] device (wlp1s0): state change: disconnected -> unmanaged (reason 'sleeping', sys-iface-state: 'managed')
Nov 27 19:33:01 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif 6e:5e:68:b8:6c:ff on port 0
Nov 27 19:33:01 x15 NetworkManager[483]: <info>  [1606476781.9369] device (wlp1s0): set-hw-addr: reset MAC address to 4C:EB:BD:95:27:03 (unmanage)
Nov 27 19:33:02 x15 kernel: rtw_8822ce 0000:01:00.0: start vif 4c:eb:bd:95:27:03 on port 0
Nov 27 19:33:02 x15 systemd[1]: Reached target Sleep.
Nov 27 19:33:02 x15 systemd[1]: Starting Suspend...
Nov 27 19:33:02 x15 systemd-sleep[89202]: Suspending system...
Nov 27 19:33:02 x15 kernel: PM: suspend entry (deep)
Nov 27 19:33:26 x15 kernel: Filesystems sync: 0.011 seconds
Nov 27 19:33:26 x15 kernel: Freezing user space processes ... (elapsed 0.002 seconds) done.
Nov 27 19:33:26 x15 kernel: OOM killer disabled.
Nov 27 19:33:26 x15 kernel: Freezing remaining freezable tasks ... (elapsed 0.001 seconds) done.
Nov 27 19:33:26 x15 kernel: printk: Suspending console(s) (use no_console_suspend to debug)
Nov 27 19:33:26 x15 kernel: rtw_8822ce 0000:01:00.0: stop vif 4c:eb:bd:95:27:03 on port 0
Nov 27 19:33:26 x15 kernel: [drm] free PSP TMR buffer
Nov 27 19:33:26 x15 kernel: ACPI: EC: interrupt blocked
Nov 27 19:33:26 x15 kernel: ACPI: Preparing to enter system sleep state S3
Nov 27 19:33:26 x15 kernel: ACPI: EC: event blocked
Nov 27 19:33:26 x15 kernel: ACPI: EC: EC stopped
Nov 27 19:33:26 x15 kernel: PM: Saving platform NVS memory
Nov 27 19:33:26 x15 kernel: Disabling non-boot CPUs ...
Nov 27 19:33:26 x15 kernel: smpboot: CPU 1 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 2 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 3 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 4 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 5 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 6 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 7 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 8 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 9 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 10 is now offline
Nov 27 19:33:26 x15 kernel: smpboot: CPU 11 is now offline
Nov 27 19:33:26 x15 kernel: ACPI: Low-level resume complete
Nov 27 19:33:26 x15 kernel: ACPI: EC: EC started
Nov 27 19:33:26 x15 kernel: PM: Restoring platform NVS memory
Nov 27 19:33:26 x15 kernel: LVT offset 0 assigned for vector 0x400
Nov 27 19:33:26 x15 kernel: Enabling non-boot CPUs ...
Nov 27 19:33:26 x15 kernel: x86: Booting SMP configuration:
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 1 APIC 0x1
Nov 27 19:33:26 x15 kernel: microcode: CPU1: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C001: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU1 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 2 APIC 0x2
Nov 27 19:33:26 x15 kernel: microcode: CPU2: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C002: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU2 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 3 APIC 0x3
Nov 27 19:33:26 x15 kernel: microcode: CPU3: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C003: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU3 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 4 APIC 0x4
Nov 27 19:33:26 x15 kernel: microcode: CPU4: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C004: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU4 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 5 APIC 0x5
Nov 27 19:33:26 x15 kernel: microcode: CPU5: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C005: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU5 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 6 APIC 0x8
Nov 27 19:33:26 x15 kernel: microcode: CPU6: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C006: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU6 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 7 APIC 0x9
Nov 27 19:33:26 x15 kernel: microcode: CPU7: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C007: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU7 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 8 APIC 0xa
Nov 27 19:33:26 x15 kernel: microcode: CPU8: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C008: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU8 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 9 APIC 0xb
Nov 27 19:33:26 x15 kernel: microcode: CPU9: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C009: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU9 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 10 APIC 0xc
Nov 27 19:33:26 x15 kernel: microcode: CPU10: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C00A: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU10 is up
Nov 27 19:33:26 x15 kernel: smpboot: Booting Node 0 Processor 11 APIC 0xd
Nov 27 19:33:26 x15 kernel: microcode: CPU11: patch_level=0x08600106
Nov 27 19:33:26 x15 kernel: ACPI: \_SB_.PLTF.C00B: Found 3 idle states
Nov 27 19:33:26 x15 kernel: CPU11 is up
Nov 27 19:33:26 x15 kernel: ACPI: Waking up from system sleep state S3
Nov 27 19:33:26 x15 kernel: ACPI: EC: interrupt unblocked
Nov 27 19:33:26 x15 kernel: ACPI: EC: event unblocked
Nov 27 19:33:26 x15 kernel: [drm] PCIE GART of 1024M enabled (table at 0x000000F400900000).
Nov 27 19:33:26 x15 kernel: [drm] PSP is resuming...
Nov 27 19:33:26 x15 kernel: [drm] reserve 0x400000 from 0xf41f800000 for PSP TMR
Nov 27 19:33:26 x15 kernel: rtw_8822ce 0000:01:00.0: start vif 4c:eb:bd:95:27:03 on port 0
Nov 27 19:33:26 x15 kernel: nvme nvme0: 16/0/0 default/read/poll queues
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: SMU is resuming...
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: dpm has been disabled
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: SMU is resumed successfully!
Nov 27 19:33:26 x15 kernel: [drm] kiq ring mec 2 pipe 1 q 0
Nov 27 19:33:26 x15 kernel: usb 1-3: reset high-speed USB device number 2 using xhci_hcd
Nov 27 19:33:26 x15 kernel: [drm] DMUB hardware initialized: version=0x01000000
Nov 27 19:33:26 x15 kernel: usb 3-3: reset full-speed USB device number 2 using xhci_hcd
Nov 27 19:33:26 x15 kernel: ata1: SATA link down (SStatus 0 SControl 300)
Nov 27 19:33:26 x15 kernel: ata2: SATA link down (SStatus 0 SControl 300)
Nov 27 19:33:26 x15 kernel: [drm] Failed to add display topology, DTM TA is not initialized.
Nov 27 19:33:26 x15 kernel: [drm] VCN decode and encode initialized successfully(under DPG Mode).
Nov 27 19:33:26 x15 kernel: [drm] JPEG decode initialized successfully.
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring gfx uses VM inv eng 0 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.0.0 uses VM inv eng 1 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.1.0 uses VM inv eng 4 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.2.0 uses VM inv eng 5 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.3.0 uses VM inv eng 6 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.0.1 uses VM inv eng 7 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.1.1 uses VM inv eng 8 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.2.1 uses VM inv eng 9 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring comp_1.3.1 uses VM inv eng 10 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring kiq_2.1.0 uses VM inv eng 11 on hub 0
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring sdma0 uses VM inv eng 0 on hub 1
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_dec uses VM inv eng 1 on hub 1
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_enc0 uses VM inv eng 4 on hub 1
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring vcn_enc1 uses VM inv eng 5 on hub 1
Nov 27 19:33:26 x15 kernel: amdgpu 0000:03:00.0: amdgpu: ring jpeg_dec uses VM inv eng 6 on hub 1
Nov 27 19:33:26 x15 kernel: acpi LNXPOWER:04: Turning OFF
Nov 27 19:33:26 x15 kernel: acpi LNXPOWER:02: Turning OFF
Nov 27 19:33:26 x15 kernel: acpi LNXPOWER:00: Turning OFF
Nov 27 19:33:26 x15 kernel: OOM killer enabled.
Nov 27 19:33:26 x15 kernel: Restarting tasks ... 
Nov 27 19:33:26 x15 kernel: pci_bus 0000:01: Allocating resources
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:02.2: bridge window [mem 0x00100000-0x000fffff 64bit pref] to [bus 01] add_size 200000 add_align 100000
Nov 27 19:33:26 x15 kernel: pci_bus 0000:02: Allocating resources
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:02.4: bridge window [io  0x1000-0x0fff] to [bus 02] add_size 1000
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:02.4: bridge window [mem 0x00100000-0x000fffff 64bit pref] to [bus 02] add_size 200000 add_align 100000
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:02.2: BAR 15: assigned [mem 0x430000000-0x4301fffff 64bit pref]
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:02.4: BAR 15: assigned [mem 0x430200000-0x4303fffff 64bit pref]
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:02.4: BAR 13: assigned [io  0x3000-0x3fff]
Nov 27 19:33:26 x15 kernel: pci_bus 0000:03: Allocating resources
Nov 27 19:33:26 x15 kernel: pci_bus 0000:04: Allocating resources
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:08.2: bridge window [io  0x1000-0x0fff] to [bus 04] add_size 1000
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:08.2: bridge window [mem 0x00100000-0x000fffff 64bit pref] to [bus 04] add_size 200000 add_align 100000
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:08.2: BAR 15: assigned [mem 0x430400000-0x4305fffff 64bit pref]
Nov 27 19:33:26 x15 kernel: pcieport 0000:00:08.2: BAR 13: assigned [io  0x4000-0x4fff]
Nov 27 19:33:26 x15 kernel: Bluetooth: hci0: RTL: examining hci_ver=0a hci_rev=000c lmp_ver=0a lmp_subver=8822
Nov 27 19:33:26 x15 kernel: Bluetooth: hci0: RTL: rom_version status=0 version=3
Nov 27 19:33:26 x15 kernel: Bluetooth: hci0: RTL: loading rtl_bt/rtl8822cu_fw.bin
Nov 27 19:33:26 x15 kernel: Bluetooth: hci0: RTL: loading rtl_bt/rtl8822cu_config.bin
Nov 27 19:33:26 x15 kernel: Bluetooth: hci0: RTL: loading rtl_bt/rtl8822cu_config.bin
Nov 27 19:33:26 x15 kernel: Bluetooth: hci0: RTL: cfg_sz 6, total sz 34338
Nov 27 19:33:26 x15 kernel: done.
Nov 27 19:33:26 x15 systemd[1]: Starting Load/Save RF Kill Switch Status...
Nov 27 19:33:26 x15 upowerd[2072]: treating change event as add on /sys/devices/pci0000:00/0000:00:08.1/0000:03:00.4/usb3/3-3
Nov 27 19:33:26 x15 systemd[1]: Stopped target Bluetooth.
Nov 27 19:33:26 x15 upowerd[2072]: treating change event as add on /sys/devices/pci0000:00/0000:00:08.1/0000:03:00.4/usb3/3-3
Nov 27 19:33:26 x15 systemd[1]: Reached target Bluetooth.
Nov 27 19:33:26 x15 systemd[1]: Started Load/Save RF Kill Switch Status.
Nov 27 19:33:26 x15 audit[1]: SERVICE_START pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=systemd-rfkill comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
Nov 27 19:33:26 x15 kernel: audit: type=1130 audit(1606476806.499:140): pid=1 uid=0 auid=4294967295 ses=4294967295 subj==unconfined msg='unit=systemd-rfkill comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res>
Nov 27 19:33:26 x15 systemd-sleep[89202]: System resumed.
Nov 27 19:33:26 x15 kernel: PM: suspend exit
```
