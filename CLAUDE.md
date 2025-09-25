# CLAUDE.md - custom-packages AI Development Context

This file provides AI development context and guidance for working with the custom-packages submodule of the AcreetionOS ARM64 workspace.

## Repository Context

**Domain**: Custom Package Development and Cross-compilation
**Primary Technologies**: PKGBUILD, makepkg, Qt/KDE frameworks, ARM64 cross-compilation
**Development Focus**: AcreetionOS-specific component compilation for ARM64 architecture

### Repository Purpose
The custom-packages repository manages AcreetionOS-specific components that differentiate the distribution from standard Arch Linux. This includes the Calamares system installer, custom libraries, system utilities, and branding elements. The primary challenge is adapting these x86_64-native packages for ARM64 architecture through cross-compilation while maintaining full functionality and AcreetionOS identity.

### Scope and Boundaries
- **In Scope**: AcreetionOS-specific packages requiring custom compilation, ARM64 architecture adaptation, installer components, system utilities
- **Out of Scope**: Standard Arch Linux packages (handled by package repositories), hardware-specific drivers (hardware-support submodule), bootloader components (boot-systems submodule)
- **Key Focus**: Qt/KDE application cross-compilation, library compatibility, package building automation

## Development Patterns

### Code Organization
```
custom-packages/
├── packagename/
│   ├── PKGBUILD              # Arch package build script
│   ├── .SRCINFO              # Package metadata
│   ├── packagename.install   # Installation hooks
│   └── arm64-patches/        # ARM64-specific patches
├── scripts/
│   ├── build-package.sh      # Individual package building
│   ├── build-all.sh          # Batch package building
│   └── validate-deps.sh      # Dependency validation
└── build-configs/
    ├── arm64.conf            # ARM64 makepkg configuration
    └── cross-compile.conf    # Cross-compilation settings
```

### File Structure Conventions
- Each package maintains standard Arch Linux packaging structure
- ARM64-specific modifications isolated in patches or conditional PKGBUILD sections
- Build scripts use consistent naming and error handling patterns
- Configuration files clearly separate cross-compilation from native build settings

### Naming Conventions
- Package directories match upstream AcreetionOS naming
- ARM64-specific files use `arm64-` prefix
- Build scripts use descriptive names with `.sh` extension
- Patch files follow `packagename-arm64-description.patch` pattern

### Common Workflows
1. **Package Analysis**: Examine x86_64 PKGBUILD for ARM64 compatibility
2. **Dependency Resolution**: Verify ARM64 availability of all dependencies
3. **Cross-compilation Setup**: Configure toolchain and target architecture
4. **Build and Test**: Compile package and validate ARM64 binary output
5. **Integration Testing**: Test package installation and functionality
6. **Documentation**: Update package metadata and build instructions

## Key Technologies

### Primary Languages
- **Bash Shell Scripting**: PKGBUILD files, build automation scripts
- **Python 3**: Build system integration, configuration management
- **C/C++**: Custom library compilation (lib2fix, system utilities)
- **Qt/QML**: Calamares installer interface components

### Frameworks and Tools
- **makepkg**: Arch Linux package building system
- **Calamares Framework**: System installer based on Qt/KDE
- **Qt5/Qt6**: Application framework for installer components
- **KDE Frameworks**: Advanced installer modules and system integration
- **CMake/Autotools**: Build system configuration for C++ packages

### Build Systems
- **Cross-compilation Toolchain**: gcc-aarch64-linux-gnu, binutils
- **Package Management**: makepkg with ARM64 configuration
- **Dependency Management**: pacman database, ARM64 repository access
- **Testing Framework**: QEMU ARM64 emulation, hardware validation

### Configuration Management
- **Build Configurations**: Architecture-specific makepkg settings
- **Cross-compilation Environment**: Sysroot, toolchain paths, target settings
- **Package Metadata**: .SRCINFO generation, dependency specification
- **Installation Hooks**: Pre/post-installation scripts for ARM64 systems

## Integration Points

### Submodule Dependencies
- **arm64-toolchain**: Cross-compilation environment setup and toolchain management
- **package-builder**: Automated build workflows and continuous integration
- **iso-builder**: Custom package inclusion in final ARM64 ISO images
- **hardware-support**: Hardware-specific configurations for custom packages

### Data Flow
```
Source Code → Cross-compilation → ARM64 Packages → Integration Testing → ISO Inclusion
     ↓              ↓                    ↓               ↓              ↓
PKGBUILD → arm64-toolchain → custom-packages → package-builder → iso-builder
```

### API Interfaces
- **Package Management**: Standard Arch Linux pacman interface compatibility
- **Installation System**: Calamares API for custom modules and configurations
- **System Integration**: systemd service definitions, XDG desktop entries
- **Hardware Abstraction**: Standard Linux kernel interfaces, device management

### Cross-Repository Communication
- **Build Coordination**: Shared build status and dependency information
- **Artifact Distribution**: Package artifacts shared via releases submodule
- **Configuration Synchronization**: Hardware-specific settings coordination
- **Testing Coordination**: Integration testing with iso-builder workflows

## Testing Strategy

### Testing Philosophy
Comprehensive testing across cross-compilation, installation, and runtime phases to ensure ARM64 packages maintain full functionality equivalent to x86_64 versions while adapting to ARM64-specific requirements.

### Test Types and Coverage
- **Build Testing**: PKGBUILD syntax validation, cross-compilation success
- **Binary Validation**: ARM64 architecture verification, library linking
- **Installation Testing**: Package installation/removal on ARM64 systems
- **Functional Testing**: Application functionality on target hardware
- **Integration Testing**: Package interaction with system components

### Automation Approaches
```bash
# Build validation pipeline
./scripts/validate-pkgbuild.sh calamares
./scripts/cross-compile-test.sh calamares --qemu
./scripts/integration-test.sh calamares --hardware=rpi4

# Continuous integration hooks
pre-commit: syntax validation, dependency checking
post-build: binary analysis, installation testing
pre-release: hardware validation, performance testing
```

### Validation Criteria
- All packages successfully cross-compile for ARM64 architecture
- Binary outputs validate as ARM64 ELF executables
- Package installation completes without errors on target systems
- Application functionality matches x86_64 equivalent behavior
- Performance meets acceptable thresholds for target hardware

## Build and Deployment

### Build Process
```bash
# Environment setup
source ../arm64-toolchain/setup-cross-env.sh
export TARGET_ARCH=aarch64
export CROSS_COMPILE=aarch64-linux-gnu-

# Individual package build
cd calamares/
makepkg -s --config ../build-configs/arm64.conf

# Batch building
./scripts/build-all.sh --arch=aarch64 --jobs=4
```

### Compilation Requirements
- ARM64 cross-compilation toolchain (gcc-aarch64-linux-gnu)
- Target architecture libraries and headers (Qt, KDE, system libraries)
- Proper sysroot configuration with ARM64 dependencies
- Build environment with sufficient resources (disk, memory)

### Deployment Considerations
- Package artifacts must maintain Arch Linux package format compatibility
- ARM64 binaries require proper library dependency resolution
- Installation hooks must handle ARM64-specific system configurations
- Package repositories must support ARM64 architecture packages

### ARM64-Specific Concerns
- **Binary Compatibility**: Ensure all compiled binaries target ARM64 architecture
- **Library Dependencies**: Verify ARM64 availability of all required libraries
- **Performance Optimization**: Apply ARM64-specific compiler optimizations
- **Hardware Integration**: Test functionality on diverse ARM64 hardware platforms

## Quality Standards

### Code Review Criteria
- **PKGBUILD Correctness**: Proper syntax, dependency specification, build process
- **ARM64 Compatibility**: Architecture-specific adaptations and optimizations
- **Build Reproducibility**: Consistent compilation results across environments
- **Error Handling**: Robust error detection and recovery in build scripts
- **Documentation Completeness**: Clear build instructions and troubleshooting guides

### Documentation Requirements
- Each package directory must include README with ARM64-specific notes
- PKGBUILD files require inline comments for ARM64 adaptations
- Build scripts need comprehensive usage documentation
- Integration points with other submodules must be documented

### Performance Standards
- Cross-compilation build times should be reasonable for development workflow
- Package size optimization for ARM64 targets with limited storage
- Runtime performance equivalent to x86_64 versions where possible
- Memory usage optimized for ARM64 hardware constraints

### Security Considerations
- All package sources verified and checksums validated
- Cross-compilation toolchain security and authenticity
- Package signing and verification for ARM64 artifacts
- Secure defaults for system installer and configuration tools

## Troubleshooting

### Common Development Issues
- **Qt library missing for ARM64**: Install qt5-base-dev:arm64 or configure cross-compilation sysroot
- **Cross-compilation toolchain errors**: Verify arm64-toolchain submodule setup and PATH configuration
- **Package dependency conflicts**: Check ARM64 package availability and version compatibility
- **Calamares build failures**: Ensure KDE Frameworks ARM64 development libraries installed

### Debug Procedures
```bash
# Cross-compilation environment validation
aarch64-linux-gnu-gcc --version
pkg-config --list-all | grep -i qt
echo $PKG_CONFIG_PATH

# Package build debugging
makepkg --config build-configs/arm64.conf --log --debug
file built-package/*.pkg.tar.xz  # Verify ARM64 architecture

# Binary analysis
objdump -f /path/to/binary
ldd /path/to/binary  # Check library dependencies
```

### Performance Analysis
- Build time profiling for optimization opportunities
- Package size analysis for storage efficiency
- Runtime performance testing on target ARM64 hardware
- Memory usage monitoring during compilation and execution

### Error Handling Patterns
```bash
# Build script error handling
set -euo pipefail

validate_arch() {
    case "${TARGET_ARCH:-}" in
        aarch64) return 0 ;;
        *) echo "Error: TARGET_ARCH must be set to aarch64" >&2; return 1 ;;
    esac
}

build_package() {
    local package="$1"
    if ! makepkg -s --config build-configs/arm64.conf; then
        echo "Build failed for $package" >&2
        return 1
    fi
}
```

## Milestone Alignment

### Current Milestone Objectives
**Milestone 0 - Infrastructure Setup**: Establish repository structure, analyze custom packages for ARM64 compatibility, plan cross-compilation approach

### Repository-Specific Goals
- Complete analysis of all custom packages for ARM64 porting requirements
- Set up cross-compilation infrastructure for Qt/KDE applications
- Create ARM64-specific build configurations and automation scripts
- Establish testing framework for package validation

### Success Criteria
- All 5 custom packages analyzed and ARM64 port complexity assessed
- Calamares installer successfully cross-compiles for ARM64 architecture
- Package build automation integrated with arm64-toolchain setup
- Documentation complete for ARM64 custom package development

### Dependencies on Other Submodules
- **arm64-toolchain**: Cross-compilation environment must be operational
- **package-builder**: Automated build workflows for integration
- **iso-builder**: Package inclusion process for ARM64 ISOs
- **testing-infrastructure**: QEMU and hardware testing capabilities

## Reference Materials

### Upstream Documentation
- [Calamares Documentation](https://calamares.io/docs/): System installer framework
- [Qt Cross-compilation Guide](https://doc.qt.io/qt-5/): Qt application cross-compilation
- [Arch Linux Packaging](https://wiki.archlinux.org/title/PKGBUILD): Package building standards
- [ARM64 Development Guide](https://developer.arm.com/): Architecture-specific optimization

### Technical Specifications
- Arch Linux Package Format specification
- Qt/KDE API documentation for installer modules
- ARM64 ABI and calling conventions
- Linux FHS (Filesystem Hierarchy Standard)

### Best Practices
- Cross-compilation best practices for Qt applications
- Package building automation and CI/CD integration
- ARM64 performance optimization techniques
- Linux distribution customization patterns

### Learning Resources
- Qt Creator ARM64 development setup guides
- Calamares module development tutorials
- ARM64 cross-compilation troubleshooting guides
- Arch Linux ARM package maintenance documentation

## Development Environment

### Required Tools
```bash
# Cross-compilation toolchain
gcc-aarch64-linux-gnu
binutils-aarch64-linux-gnu
libc6-dev-arm64-cross

# Qt/KDE development for ARM64
qt5-qmake:arm64
qt5-default:arm64
kde-frameworks-dev:arm64

# Build and packaging tools
makepkg
git
cmake
python3
```

### Configuration Files
- **build-configs/arm64.conf**: makepkg configuration for ARM64 builds
- **build-configs/cross-compile.conf**: Cross-compilation environment settings
- **.cross-compile-env**: Environment variables for cross-compilation
- **scripts/setup-build-env.sh**: Automated environment setup script

### Environment Variables
```bash
export TARGET_ARCH=aarch64
export CROSS_COMPILE=aarch64-linux-gnu-
export PKG_CONFIG_PATH=/usr/lib/aarch64-linux-gnu/pkgconfig
export CMAKE_TOOLCHAIN_FILE=../arm64-toolchain/cmake/arm64-toolchain.cmake
```

### IDE/Editor Setup
- **VSCode**: Extensions for PKGBUILD syntax, ARM64 debugging support
- **Qt Creator**: ARM64 cross-compilation kit configuration
- **Vim/Neovim**: Syntax highlighting for PKGBUILD files
- **Build Integration**: Makefile/CMake integration for cross-compilation

## AI Assistance Guidelines

### Preferred Code Patterns
```bash
# PKGBUILD ARM64 adaptation pattern
arch=('aarch64')
if [[ $CARCH == "aarch64" ]]; then
    depends+=('arm64-specific-package')
    # ARM64-specific build configuration
fi

# Cross-compilation detection
if [[ -n "${CROSS_COMPILE:-}" ]]; then
    export CC="${CROSS_COMPILE}gcc"
    export CXX="${CROSS_COMPILE}g++"
fi
```

### Architecture Principles
- Maintain compatibility with standard Arch Linux packaging patterns
- Isolate ARM64-specific modifications for maintainability
- Prioritize cross-compilation over native ARM64 building for development speed
- Follow upstream AcreetionOS customization patterns

### Documentation Style
- Inline comments in PKGBUILD files explaining ARM64 modifications
- README files with clear ARM64 build instructions and requirements
- Troubleshooting sections with hardware-specific considerations
- Examples showing both x86_64 and ARM64 build commands

### Review Focus Areas
- ARM64 binary compatibility and architecture verification
- Package dependency availability and version compatibility
- Cross-compilation correctness and reproducibility
- Integration testing with target ARM64 hardware platforms

## Project Context

This repository operates within the AcreetionOS ARM64 multi-repository workspace:

- **Main Workspace**: [acreetionos-arm64/workspace](https://github.com/acreetionos-arm64/workspace)
- **Architecture**: Distributed development across 10 specialized repositories
- **Coordination**: Git submodules with independent development lifecycles
- **Timeline**: Sustainable side project, 18-36 month development cycle

## Upstream Relationships

### AcreetionOS Integration
Custom packages maintain compatibility with upstream AcreetionOS while adapting for ARM64 architecture. Changes focus on build system adaptation rather than functionality modification to preserve AcreetionOS identity and user experience.

### GitLab CE Coordination
Planned coordination with AcreetionOS upstream through GitLab CE mirroring for source synchronization, custom package updates, and contribution back to upstream project for ARM64 support improvements.

### Community Contribution
ARM64 adaptations and cross-compilation improvements contributed back to Calamares project, Qt/KDE communities, and Arch Linux ARM for broader ecosystem benefit and sustainability.

---

*This AI context file is maintained to provide effective development assistance specific to the custom-packages domain within the AcreetionOS ARM64 project.*