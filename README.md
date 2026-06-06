# OpenCore EFI for E3-1225 V2

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

E3-1225 V2 + H61 平台黑苹果 OpenCore 引导 EFI。已成功安装 **macOS Big Sur**。

## 硬件配置

| 组件 | 型号 | 状态 |
|------|------|------|
| CPU | Intel Xeon E3-1225 V2 (Ivy Bridge) | ✅ |
| iGPU | Intel HD Graphics P4000 | ✅ WhateverGreen 驱动 |
| 主板 | H61 芯片组 | ✅ |
| 网卡 | Realtek RTL8111 | ✅ |
| 无线 | Intel 无线网卡 | ✅ AirportItlwm |
| USB | H61 芯片组 | ✅ 已定制 USBMap |

## OpenCore 版本

- **SMBIOS**: iMac15,1
- **macOS**: Big Sur

## Kexts 列表

- [Lilu](https://github.com/acidanthera/Lilu) — 内核补丁
- [VirtualSMC](https://github.com/acidanthera/VirtualSMC) — SMC 模拟
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen) — iGPU 修复
- [AppleALC](https://github.com/acidanthera/AppleALC) — 音频
- [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) — Intel 无线网卡
- [IntelMausi](https://github.com/acidanthera/IntelMausi) — Intel 有线网卡
- [RealtekRTL8111](https://github.com/RehabMan/OS-X-Realtek-Network) — 板载网卡
- [SMCProcessor](https://github.com/acidanthera/VirtualSMC) — CPU 传感器
- [SMCSuperIO](https://github.com/acidanthera/VirtualSMC) — 主板传感器
- [Innie](https://github.com/cdf/Innie) — 内建磁盘标识
- [USBMap](https://github.com/corpnewt/USBMap) — USB 定制

## 使用说明

1. 将 EFI 文件夹复制到 ESP 分区
2. 在 BIOS 中启用集显（IGPU），设置显存不低于 64MB
3. 使用 OpenCore 引导启动

### 注意

如果你的机型是 E3-1225 V2 且使用 H61 主板，可直接使用此 EFI。其他主板可能需要调整 ACPI 和 USB 配置。

## 已知问题

- 仅支持 macOS Big Sur（如需更新版本可能需要更新 kexts 和 OC 版本）
- H61 主板 SATA 接口限制（建议使用 SATA SSD）

## 鸣谢

- [OpenCore 官方](https://github.com/acidanthera/OpenCorePkg)
- [acidanthera 团队](https://github.com/acidanthera)
- [OpenIntelWireless](https://github.com/OpenIntelWireless)
