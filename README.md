# custom-packages

> AcreetionOS-specific components and branding for ARM64 architecture

## Repository Purpose

This repository contains AcreetionOS-specific custom packages that differentiate the distribution from standard Arch Linux. These components provide the unique AcreetionOS identity, branding, and functionality, requiring specialized ARM64 compilation and adaptation.

This repository is part of the AcreetionOS ARM64 multi-repository workspace, providing custom package compilation and management for the overall ARM64 port project.

## Architecture Context

custom-packages integrates with the AcreetionOS ARM64 workspace as follows:

- **Primary Role**: AcreetionOS-specific component compilation for ARM64
- **Dependencies**: arm64-toolchain (cross-compilation), package-builder (automation)
- **Integration Points**: iso-builder (package inclusion), releases (artifact distribution)
- **Upstream Relationship**: AcreetionOS upstream packages + ARM64 cross-compilation

### Related Repositories
- **[iso-builder](https://github.com/acreetionos-arm64/iso-builder)** - Includes compiled custom packages in ISO
- **[package-builder](https://github.com/acreetionos-arm64/package-builder)** - Automated compilation workflows
- **[arm64-toolchain](https://github.com/acreetionos-arm64/arm64-toolchain)** - Cross-compilation environment
- **[releases](https://github.com/acreetionos-arm64/releases)** - Package artifact distribution
- **[hardware-support](https://github.com/acreetionos-arm64/hardware-support)** - Hardware-specific configurations

## Development Status

**Current Milestone**: Milestone 0 - Infrastructure Setup
**Completion Status**: 🔄 x86_64 → ARM64 compilation planning in progress

### Milestone Progress
- ✅ Repository structure established
- 🔄 ARM64 compilation requirements analysis
- 📋 Planned: Calamares installer ARM64 adaptation
- 📋 Planned: Custom library cross-compilation
- 📋 Planned: Package building automation integration

## Quick Start Guide

### Prerequisites
- ARM64 cross-compilation toolchain installed
- Access to AcreetionOS package repositories
- Qt5/6 development libraries for ARM64
- Build dependencies for each custom package

### Setup Instructions
```bash
# Clone with workspace context
cd /path/to/acreetionos-arm64/workspace
git submodule update --init --recursive
cd custom-packages/

# Set up ARM64 cross-compilation environment
# (Requires arm64-toolchain submodule setup)
source ../arm64-toolchain/setup-cross-env.sh

# Validate package build dependencies
./scripts/validate-dependencies.sh
```

### Basic Usage
```bash
# Build individual package for ARM64
./build-package.sh calamares-config

# Build all packages
./build-all.sh

# Test package in ARM64 environment
./test-package.sh calamares-config --qemu
```

## Directory Structure

```
custom-packages/
├── calamares/              # System installer
├── calamares-config/       # Installer configuration
├── calamares-config-net/   # Network installer config
├── lib2fix/               # Compatibility libraries
├── updater/               # System update tools
├── scripts/               # Build and test scripts
├── arm64-patches/         # ARM64-specific patches
└── build-configs/         # ARM64 build configurations
```

### Key Files and Directories
- **Individual package directories**: Each contains PKGBUILD, sources, and ARM64 adaptations
- **scripts/**: Build automation, dependency validation, testing utilities
- **arm64-patches/**: Architecture-specific patches for ARM64 compatibility
- **build-configs/**: Cross-compilation configurations and toolchain settings

## Current Custom Packages Status

### System Components
- **`calamares/`** - System installer based on Calamares
- **`calamares-config/`** - Calamares installer configuration
- **`calamares-config-net/`** - Network installer configuration
- **`lib2fix/`** - Compatibility library fixes
- **`updater/`** - System update and configuration tools

### System Components
- **`calamares/`** - System installer based on Calamares framework
- **`calamares-config/`** - AcreetionOS-specific installer configuration
- **`calamares-config-net/`** - Network installation configuration
- **`lib2fix/`** - Compatibility library fixes and patches
- **`updater/`** - System update and configuration management tools

## ARM64 Port Progress

| Package | x86_64 Status | ARM64 Status | Complexity |
|---------|---------------|--------------|------------|
| `calamares` | ✅ Complete | 🔄 Planning | High - Qt/KDE framework |
| `calamares-config` | ✅ Complete | 🔄 Planning | Medium - Configuration only |
| `calamares-config-net` | ✅ Complete | 🔄 Planning | Medium - Configuration only |
| `lib2fix` | ✅ Complete | 🔄 Planning | Medium - Library compilation |
| `updater` | ✅ Complete | 🔄 Planning | Low - Script-based |

## Development Workflow

### Making Changes
1. **Select target package**: Choose from calamares, lib2fix, updater, etc.
2. **Set up cross-compilation**: Source ARM64 toolchain environment
3. **Modify PKGBUILD**: Adapt for ARM64 architecture and dependencies
4. **Apply ARM64 patches**: Use patches from `arm64-patches/` directory
5. **Test build process**: Validate compilation and packaging
6. **Integration testing**: Test with iso-builder inclusion

### Testing Changes
- **QEMU testing**: Validate packages in ARM64 emulation
- **Hardware testing**: Test on Raspberry Pi 4/5 or other ARM64 devices
- **Integration testing**: Verify compatibility with other custom packages
- **Installer testing**: Validate Calamares functionality if modified

### Contributing
1. Focus on Qt/KDE ARM64 compatibility for Calamares components
2. Ensure library cross-compilation correctness for lib2fix
3. Test package installations on target ARM64 hardware
4. Document ARM64-specific build requirements and dependencies

## Dependencies

### System Requirements
- Linux build environment (x86_64 host recommended)
- ARM64 cross-compilation toolchain (gcc-aarch64-linux-gnu)
- Qt5/Qt6 development libraries for ARM64
- CMake, make, and standard build tools

### External Dependencies
- **Calamares Framework**: Qt-based installer framework
- **KDE Frameworks**: Required for advanced Calamares modules
- **Python 3**: For build scripts and configuration tools
- **Git**: For source management and patch application

### Submodule Dependencies
- **arm64-toolchain**: Cross-compilation environment setup
- **package-builder**: Automated build workflows
- **iso-builder**: Package inclusion in final ISO images
- **hardware-support**: Hardware-specific configurations

## Testing Instructions

### Unit Testing
- Individual package build validation
- PKGBUILD syntax and dependency checking
- ARM64 binary compatibility verification
- Package installation and removal testing

### Integration Testing
- Cross-package dependency resolution
- Calamares installer functionality validation
- System update and configuration tool testing
- Hardware-specific feature testing

### Validation
- ARM64 architecture compatibility verification
- Performance testing on target hardware platforms
- User experience testing for AcreetionOS-specific features
- Regression testing against x86_64 functionality

## Build/Deployment

### Build Process
```bash
# Set up environment
source ../arm64-toolchain/setup-cross-env.sh

# Build specific package
cd calamares/
makepkg -s --config ../build-configs/arm64.conf

# Build all packages
cd ..
./build-all.sh --arch=aarch64
```

### Compilation Requirements
- ARM64 cross-compilation toolchain properly configured
- Target architecture set to aarch64 in all PKGBUILD files
- Qt/KDE libraries available for ARM64 target
- Proper sysroot configuration for ARM64 dependencies

### Deployment Considerations
- Package artifacts stored in releases submodule
- Integration with AcreetionOS ARM64 package repositories
- Hardware-specific package variants for different ARM64 platforms
- Version synchronization with upstream AcreetionOS packages

## Upstream Coordination

### GitLab CE Mirroring
- **Status**: 📋 Planned coordination with AcreetionOS upstream
- **Purpose**: Custom package source synchronization
- **Approach**: GitLab CE integration through upstream-sync submodule

### Upstream Relationships
- **AcreetionOS**: Source packages and configuration templates
- **Calamares Project**: Installer framework updates and compatibility
- **Arch Linux ARM**: ARM64 packaging best practices and dependencies

### Contribution Upstream
- ARM64 compatibility improvements contributed back to AcreetionOS
- Calamares ARM64 support contributions to upstream project
- Documentation and guides for ARM64 custom package development

## Troubleshooting

### Common Issues
- **Qt library not found for ARM64**: Install qt5-base-dev:arm64 or equivalent
- **Cross-compilation failures**: Verify arm64-toolchain setup and sysroot
- **Missing dependencies**: Check ARM64 package availability in repositories
- **Calamares build errors**: Ensure KDE Frameworks ARM64 libraries installed

### Debug Information
```bash
# Check cross-compilation environment
aarch64-linux-gnu-gcc --version
pkg-config --list-all | grep -i qt

# Validate ARM64 dependencies
ldd /path/to/built/binary
file /path/to/built/binary  # Should show aarch64

# Debug package building
makepkg --config build-configs/arm64.conf --log
```

### Getting Help
- **GitHub Issues**: Use repository-specific issue templates
- **Documentation**: Check CLAUDE.md for development context
- **Upstream Resources**: Calamares documentation, Qt ARM64 guides
- **Community**: AcreetionOS ARM64 project discussions

## Project Context

- **AcreetionOS ARM64 Workspace**: [Main Repository](https://github.com/acreetionos-arm64/workspace)
- **Project Documentation**: [Documentation Repository](https://github.com/acreetionos-arm64/documentation)
- **Issue Tracking**: [GitHub Issues](https://github.com/acreetionos-arm64/custom-packages/issues)
- **Development Coordination**: See [CLAUDE.md](./CLAUDE.md) for AI development context

## License

MIT License (organization repositories) + GPL-3.0 (inherited from AcreetionOS upstream)

See individual package directories for specific licensing information.

---

*This repository is part of the AcreetionOS ARM64 port project - a sustainable side project focused on learning Linux distribution development while creating a functional ARM64 port of AcreetionOS.*