# AcreetionOS Custom Packages - ARM64 Port

**Repository**: [`acreetionos-arm64/custom-packages`](https://github.com/acreetionos-arm64/custom-packages)
**Purpose**: AcreetionOS-specific components and branding for ARM64 architecture
**Status**: x86_64 → ARM64 compilation in progress

## Overview

This repository contains AcreetionOS-specific custom packages that differentiate the distribution from standard Arch Linux. These components provide the unique AcreetionOS identity, branding, and functionality.

## Current Custom Packages (x86_64)

### System Components
- **`calamares/`** - System installer based on Calamares
- **`calamares-config/`** - Calamares installer configuration
- **`calamares-config-net/`** - Network installer configuration
- **`lib2fix/`** - Compatibility library fixes
- **`updater/`** - System update and configuration tools

## ARM64 Conversion Status

| Package | x86_64 Status | ARM64 Status | Complexity |
|---------|---------------|--------------|------------|
| `calamares` | ✅ Complete | 🔄 Planning | High - Qt/KDE framework |
| `calamares-config` | ✅ Complete | 🔄 Planning | Medium - Configuration only |
| `calamares-config-net` | ✅ Complete | 🔄 Planning | Medium - Configuration only |
| `lib2fix` | ✅ Complete | 🔄 Planning | Medium - Library compilation |
| `updater` | ✅ Complete | 🔄 Planning | Low - Script-based |

## Multi-Repository Context

Part of the [`acreetionos-arm64`](https://github.com/acreetionos-arm64) organization:
- **[workspace](https://github.com/acreetionos-arm64/workspace)** - Main coordination
- **[custom-packages](https://github.com/acreetionos-arm64/custom-packages)** - This repository
- **[iso-builder](https://github.com/acreetionos-arm64/iso-builder)** - Main build system
- **[arm64-toolchain](https://github.com/acreetionos-arm64/arm64-toolchain)** - Cross-compilation tools
- **[package-builder](https://github.com/acreetionos-arm64/package-builder)** - Package building automation

## Contributing

Focus areas for ARM64 port:
- Qt/KDE ARM64 builds for Calamares installer
- Library cross-compilation for compatibility layers
- Package building automation for ARM64 architecture
- Hardware testing on ARM64 devices

## Links

- **AcreetionOS Upstream**: [gitlab.acreetionos.org](https://gitlab.acreetionos.org)
- **Calamares Project**: [calamares.io](https://calamares.io)
- **Arch Linux ARM**: [archlinuxarm.org](https://archlinuxarm.org)