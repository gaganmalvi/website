---
title: "Amazfit Verge Internals"
date: 2022-08-16T21:06:01+05:30
draft: false
---

### About the watch
The Amazfit Verge is a smartwatch that is running a custom version of Android 5.1 Lollypop (LMY47D) running on a custom MIPS chipset manufactured by Ingenic.
This runs on the Linux 3.10.14 kernel, which is pretty common for a device running Lollypop. I do not understand why Lollypop was taken as a base for the watch, since it was launched pretty recently, Feb 2019 (or so websites say). 

### CPU Info
As `/proc/cpuinfo` states
```
system type		: qogirUS
machine			: Unknown
processor		: 0
cpu model		: Ingenic Xburst V4.15  FPU V0.0
BogoMIPS		: 1030.55
wait instruction	: yes
microsecond timers	: no
tlb_entries		: 32
extra interrupt vector	: yes
hardware watchpoint	: yes, count: 1, address/irw mask: [0x0fff]
isa			: mips32r1
ASEs implemented	:
shadow register sets	: 1
kscratch registers	: 0
core			: 0
VCED exceptions		: not available
VCEI exceptions		: not available

Hardware		: watch
Serial			: 00000000 00000000 00000000 00000000
```

### `getprop` output
This will tell us all the properties of the watch that are used in runtime.

```
[dalvik.vm.dex2oat-Xms]: [64m]
[dalvik.vm.dex2oat-Xmx]: [512m]
[dalvik.vm.heapgrowthlimit]: [64m]
[dalvik.vm.heapmaxfree]: [2m]
[dalvik.vm.heapminfree]: [512k]
[dalvik.vm.heapsize]: [384m]
[dalvik.vm.heapstartsize]: [64m]
[dalvik.vm.heaptargetutilization]: [0.75]
[dalvik.vm.image-dex2oat-Xms]: [64m]
[dalvik.vm.image-dex2oat-Xmx]: [64m]
[dalvik.vm.isa.mips.features]: [default]
[dalvik.vm.stack-trace-file]: [/data/anr/traces.txt]
[debug.force_rtl]: [0]
[dev.bootcomplete]: [1]
[hm.privacy.ble.bdaddr]: []
[huami_launcher_boot_complete]: [true]
[init.svc.adbd]: [running]
[init.svc.bootanim]: [stopped]
[init.svc.console]: [running]
[init.svc.debuggerd]: [running]
[init.svc.delay_sleep]: [stopped]
[init.svc.flash_recovery]: [stopped]
[init.svc.gpsd]: [running]
[init.svc.hardware_adapter]: [stopped]
[init.svc.healthd]: [running]
[init.svc.installd]: [running]
[init.svc.iptinit]: [stopped]
[init.svc.keystore]: [running]
[init.svc.lmkd]: [running]
[init.svc.logd]: [running]
[init.svc.logwatch]: [running]
[init.svc.media]: [running]
[init.svc.netd]: [running]
[init.svc.sdcard]: [running]
[init.svc.servicemanager]: [running]
[init.svc.surfaceflinger]: [running]
[init.svc.ueventd]: [running]
[init.svc.voicewakeup]: [restarting]
[init.svc.vold]: [running]
[init.svc.wpa_supplicant]: [stopped]
[init.svc.zygote]: [running]
[media.stagefright.use-awesome]: [true]
[mihome_ble_service_is_ready]: [false]
[mihome_register_ble_service]: [true]
[net.bt.name]: [Android]
[net.change]: [net.qtaguid_enabled]
[net.hostname]: [Amazfit_Verge_0F58]
[net.qtaguid_enabled]: [1]
[net.tcp.default_init_rwnd]: [60]
[persist.sys.country]: [US]
[persist.sys.dalvik.vm.lib.2]: [libart.so]
[persist.sys.gps.agps]: [2022-08-16T00:00:00Z,2]
[persist.sys.gps.version]: [18344,eb2f43f,126]
[persist.sys.language]: [en]
[persist.sys.localevar]: []
[persist.sys.profiler_ms]: [0]
[persist.sys.timezone]: [Asia/Calcutta]
[persist.sys.usb.config]: [mtp,adb]
[prop.launcher.at_watchface]: [false]
[ro.allow.mock.location]: [0]
[ro.appwidgetserver.disable]: [true]
[ro.audio.dmic.gain.level]: [12]
[ro.baseband]: [unknown]
[ro.board.platform]: [M200]
[ro.boot.battery_type]: [1]
[ro.boot.fastboot_unlock]: [0]
[ro.boot.revision]: [00000000]
[ro.bootloader]: [unknown]
[ro.bootmode]: [unknown]
[ro.bootrotation]: [180]
[ro.bt.bdaddr]: [D8:80:3C:32:88:B5]
[ro.bt.bdaddr_path]: [/data/misc/bluetooth/bt_addr]
[ro.build.characteristics]: [tablet]
[ro.build.cmiit.id]: [XXXXXXXX]
[ro.build.date.utc]: [1587351322]
[ro.build.date]: [Mon Apr 20 10:55:22 CST 2020]
[ro.build.description]: [qogirUS-user 5.1 LMY47D 51 release-keys]
[ro.build.display.id]: [qogirUS-3.2.7.5]
[ro.build.fingerprint]: [Huami/qogirUS/watch:5.1/LMY47D/51:user/release-keys]
[ro.build.flavor]: [qogirUS-user]
[ro.build.host]: [vm10-254-107-57.ksc.com]
[ro.build.huami.model]: [A1811]
[ro.build.huami.number]: [00]
[ro.build.id]: [LMY47D]
[ro.build.oversea]: [1]
[ro.build.product]: [watch]
[ro.build.tags]: [release-keys]
[ro.build.type]: [user]
[ro.build.user]: [ubuntu]
[ro.build.version.all_codenames]: [REL]
[ro.build.version.codename]: [REL]
[ro.build.version.incremental]: [51]
[ro.build.version.release]: [5.1]
[ro.build.version.sdk]: [22]
[ro.carrier]: [wifi-only]
[ro.com.android.dateformat]: [MM-dd-yyyy]
[ro.config.alarm_alert]: [AlarmsCarbon_48k.ogg]
[ro.config.auto_brightness]: [decrease]
[ro.config.low_ram]: [true]
[ro.config.notification_sound]: [pixiedust.ogg]
[ro.config.ringtone]: [Atria.ogg]
[ro.crypto.fuse_sdcard]: [true]
[ro.crypto.state]: [unencrypted]
[ro.dalvik.vm.native.bridge]: [0]
[ro.debuggable]: [0]
[ro.enableAppScreenshot]: [0]
[ro.factorytest]: [0]
[ro.hardware]: [watch]
[ro.idlemaintenance.disable]: [true]
[ro.nfc.port]: [I2C]
[ro.opengles.version]: [131072]
[ro.product.board]: [watch]
[ro.product.brand]: [Huami]
[ro.product.cpu.abi2]: [mips]
[ro.product.cpu.abi]: [mips]
[ro.product.cpu.abilist32]: [mips,mips]
[ro.product.cpu.abilist64]: []
[ro.product.cpu.abilist]: [mips,mips]
[ro.product.device]: [watch]
[ro.product.locale.language]: [en]
[ro.product.locale.region]: [US]
[ro.product.manufacturer]: [Huami]
[ro.product.model]: [Amazfit Verge]
[ro.product.name]: [qogirUS]
[ro.revision]: [00000000]
[ro.runtime.firstboot]: [1659546324286]
[ro.secure]: [1]
[ro.serialno]: [18111914067553]
[ro.sf.lcd_density]: [240]
[ro.sn.serial_numbers]: [18111914067553]
[ro.sn.serial_numbers_factory]: [902017086C3]
[ro.wifi.channels]: [13]
[ro.wifi.wifiaddr]: [D8:80:3C:34:0F:58]
[ro.zygote]: [zygote32]
[selinux.reload_policy]: [1]
[service.adb.tcp.port]: [5555]
[service.bootanim.exit]: [1]
[sys.boot_completed]: [1]
[sys.disable.launcherhome]: [0]
[sys.fastboot.enable]: [false]
[sys.kernel.slpt.watchmode]: [1]
[sys.settings_global_version]: [37]
[sys.settings_secure_version]: [571]
[sys.settings_system_version]: [772]
[sys.slpt.lan]: [2]
[sys.state.powerlow]: [false]
[sys.sysctl.extra_free_kbytes]: [1518]
[sys.uboot.charginglevel]: [82]
[sys.uboot.chargingtime]: [1659546247000]
[sys.usb.config]: [mtp,adb]
[sys.usb.state]: [mtp,adb]
[sys.watchface.slpt.index]: [0]
[sys.watchface.sport.order]: []
[system_init.startmotioneyes]: [1]
[vold.post_fs_data_done]: [1]
[wifi.interface]: [wlan0]
[wifi.supplicant_scan_interval]: [15]
[wlan.driver.status]: [unloaded]
```
### Access into the watch
You do get a fairly simple shell once you run `adb shell`; USB debugging is allowed by default, which is a pretty funny thing considering this is a production Android device (albeit not marketed like one). 