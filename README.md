# OpenCore EFI for E3-1225 V2

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

E3-1225 V2 (Ivy Bridge) + H61 平台黑苹果 OpenCore 引导 EFI，已成功安装 **macOS Big Sur**。

## 硬件配置

| 组件 | 型号 | 状态 |
|------|------|------|
| CPU | Intel Xeon E3-1225 V2 (Ivy Bridge, 4C/4T) | ✅ |
| iGPU | Intel HD Graphics P4000 | ✅ WhateverGreen 驱动 |
| 主板 | H61 芯片组 | ✅ |
| 内存 | DDR3 | ✅ |
| 网卡 | Realtek RTL8111 | ✅ |
| 无线 | Intel 无线网卡 | ✅ AirportItlwm |
| 声卡 | Realtek ALC | ✅ AppleALC |
| USB | H61 芯片组 | ✅ 已定制 USBMap |

## OpenCore 版本

| 项目 | 值 |
|------|------|
| SMBIOS | iMac15,1 |
| 目标 macOS | Big Sur (11.x) |

## Kexts 列表

| Kext | 版本 | 功能 |
|------|------|------|
| [Lilu](https://github.com/acidanthera/Lilu) | latest | 内核扩展补丁框架 |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC) | latest | SMC 模拟 |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen) | latest | GPU/iGPU 修复（HD P4000） |
| [AppleALC](https://github.com/acidanthera/AppleALC) | latest | 板载音频 |
| [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) | latest | Intel 无线网卡 |
| [IntelMausi](https://github.com/acidanthera/IntelMausi) | latest | Intel 有线网卡 |
| [RealtekRTL8111](https://github.com/RehabMan/OS-X-Realtek-Network) | latest | 板载 Realtek 网卡 |
| [SMCProcessor](https://github.com/acidanthera/VirtualSMC) | latest | CPU 温度传感器 |
| [SMCSuperIO](https://github.com/acidanthera/VirtualSMC) | latest | 主板传感器 |
| [Innie](https://github.com/cdf/Innie) | latest | 内建磁盘标识 |
| [USBMap](https://github.com/corpnewt/USBMap) | custom | USB 端口定制 |

## BIOS 设置

进入 BIOS（开机按 Del/F2），确保以下设置：

| 选项 | 设置 |
|------|------|
| Secure Boot | **Disabled** |
| Fast Boot | **Disabled** |
| CSM | **Disabled** (UEFI only) |
| SATA Mode | **AHCI** |
| VT-d | **Disabled** |
| XHCI Hand-off | **Enabled** |
| iGPU 显存 | **64MB 或以上** |
| iGPU 优先 | **Enabled**（优先使用集显输出） |
| HPET | **Enabled** |
| Execute Disable Bit | **Enabled** |

## 首次使用

### 1. 生成唯一 SMBIOS

此 EFI 使用 **iMac15,1** SMBIOS，序列号等需要自行生成：

```bash
# 使用 GenSMBIOS 工具
# https://github.com/corpnewt/GenSMBIOS
# 选择 iMac15,1，生成 Serial / Board Serial / SmUUID / ROM
```

将生成的数值填入 `config.plist` → `PlatformInfo → Generic`：
- `SystemProductName`: iMac15,1
- `SystemSerialNumber`: (生成的序列号)
- `MLB`: (生成的 Board Serial)
- `SystemUUID`: (生成的 SmUUID)
- `ROM`: (网卡 MAC 地址)

### 2. 制作安装盘

```bash
# 在 macOS 下制作安装盘
# 下载 Big Sur 安装程序
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyUSB

# 将 EFI 复制到 USB 的 ESP 分区
```

### 3. 安装 macOS

1. 插入制作好的安装 U 盘
2. 开机选择 UEFI 启动 → OpenCore
3. 选择 `Install macOS Big Sur`
4. 安装完成后将 EFI 复制到本地硬盘的 ESP 分区

## 注意事项

- **如果你的主板不是 H61**，可能需要调整 ACPI（SSDT）和 USB 配置
- E3-1225 V2 的 HD Graphics P4000 是 Ivy Bridge 架构，不是 Haswell，请勿使用错误的 iGPU 驱动配置
- 此配置仅针对 **Big Sur** 优化，升级到更高版本 macOS 可能需要更新 kexts 和 OC
- **显示器必须插在主板的集显接口**（VGA/DVI/HDMI），不要插独显
- 升级 OpenCore 或 kexts 前备份当前 EFI

## 已知问题

- H61 主板仅有 SATA 2.0 接口（3Gbps），建议使用 SATA SSD
- HVEC 硬件编解码不支持（Ivy Bridge 架构限制）
- 隔空投送 (AirDrop) 需更换原生苹果无线网卡
- 睡眠/唤醒功能正常
- 部分 H61 主板可能需要额外 SSDT 修复

## 文件说明

| 文件 | 说明 |
|------|------|
| `EFI/OC/config.plist` | 当前使用的配置文件 |
| `EFI/OC/oldConfig.plist` | 旧版配置备份 |
| `README.md` | 本文件 |

> ~~`Config - 副本.plist`~~ ❌ 已清理（旧版备份残留）

## 更新 EFI

```bash
# 使用 OC Auxiliary Tools 或 ProperTree 编辑 config.plist
# 定期更新 kexts 和 OpenCore.efi
# 更新后用 OCValidate 检查
```

## 鸣谢

- [OpenCore 官方](https://github.com/acidanthera/OpenCorePkg)
- [acidanthera 团队](https://github.com/acidanthera)
- [OpenIntelWireless](https://github.com/OpenIntelWireless)
- [Dortania 黑苹果安装指南](https://dortania.github.io/OpenCore-Install-Guide/)
