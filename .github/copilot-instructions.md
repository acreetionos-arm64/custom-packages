# GitHub Copilot Instructions - custom-packages

This file provides GitHub Copilot with domain-specific context for the custom-packages submodule of the AcreetionOS ARM64 project.

## Repository Domain

**Specialization**: Custom Package Development and ARM64 Cross-compilation
**Primary Function**: AcreetionOS-specific component compilation for ARM64
**Technical Domain**: PKGBUILD development, Qt/KDE cross-compilation, package management

## Code Style and Conventions

### Language-Specific Guidelines
- **PKGBUILD**: Follow Arch Linux packaging standards with ARM64 adaptations
- **Bash Scripts**: POSIX-compliant with comprehensive error handling
- **Python**: PEP 8 compliant for build automation and configuration tools
- **C/C++**: Standard conventions with ARM64-specific compiler optimizations
- **CMake**: Modern CMake practices with cross-compilation toolchain files

### Coding Standards
- All shell scripts include proper error handling (`set -euo pipefail`)
- PKGBUILD files clearly separate ARM64-specific modifications
- Cross-compilation settings isolated in dedicated configuration files
- Build scripts provide verbose output for debugging cross-compilation issues
- Version pinning for critical dependencies to ensure reproducible builds

### File Organization
```
package-name/
├── PKGBUILD                 # Main package build script
├── .SRCINFO                 # Generated package metadata
├── package.install          # Installation hooks
├── arm64-patches/           # Architecture-specific patches
├── cross-compile.patch      # Cross-compilation modifications
└── README-arm64.md          # ARM64-specific build notes
```

### Naming Conventions
- Package directories match upstream AcreetionOS naming exactly
- ARM64-specific files use `arm64-` prefix consistently
- Patch files: `packagename-arm64-description.patch`
- Build scripts: `build-packagename.sh`, `test-packagename.sh`
- Configuration files: `packagename-arm64.conf`

## Framework Preferences

### Preferred Approaches
- **Cross-compilation over native**: Build on x86_64 host targeting ARM64
- **Upstream compatibility**: Maintain AcreetionOS package structure and functionality
- **Conditional compilation**: Use architecture detection for ARM64-specific code paths
- **Modular patches**: Separate patches for different ARM64 adaptations

### Architecture Patterns
```bash
# PKGBUILD architecture detection pattern
if [[ $CARCH == "aarch64" ]]; then
    # ARM64-specific configuration
    depends+=('arm64-specific-deps')
    makedepends+=('aarch64-linux-gnu-gcc')
    configure_args="--host=aarch64-linux-gnu"
else
    # x86_64 configuration (fallback)
    configure_args="--host=x86_64-linux-gnu"
fi
```

### Design Principles
- **Separation of Concerns**: Architecture-specific code isolated from common functionality
- **Build Reproducibility**: Identical builds across different development environments
- **Error Transparency**: Clear error messages for cross-compilation failures
- **Documentation First**: ARM64 adaptations documented before implementation

### Technology Stack
- **makepkg**: Core package building with ARM64 configuration
- **Qt5/Qt6**: Cross-compiled for ARM64 with proper toolchain setup
- **CMake/Autotools**: Build system configuration for cross-compilation
- **GCC Cross-compiler**: aarch64-linux-gnu-gcc toolchain

## Testing Approaches

### Unit Testing Strategy
```bash
# Package build validation
validate_pkgbuild() {
    namcap PKGBUILD || return 1
    shellcheck *.sh || return 1
    # ARM64-specific validations
    grep -q "aarch64" PKGBUILD || {
        echo "Warning: No ARM64 architecture specified"
    }
}
```

### Integration Testing
- **QEMU Testing**: Validate packages in ARM64 emulation environment
- **Hardware Testing**: Test installation on Raspberry Pi 4/5, Pine64
- **Cross-package Testing**: Verify compatibility between custom packages
- **Installer Testing**: Validate Calamares functionality with custom packages

### Test Structure
```bash
#!/bin/bash
# Test framework pattern
setup_test_environment() {
    # Set up ARM64 cross-compilation
    source ../arm64-toolchain/setup-env.sh
}

test_package_build() {
    local package="$1"
    cd "$package" || return 1
    makepkg -s --config ../build-configs/arm64.conf
}

validate_arm64_binary() {
    local binary="$1"
    file "$binary" | grep -q "aarch64" || return 1
    ldd "$binary" 2>/dev/null | grep -v "not found" || return 1
}
```

### Mocking and Fixtures
- Mock ARM64 hardware environments for package testing
- Use Docker containers with ARM64 emulation for isolation
- Create minimal test installations for dependency validation
- Generate test data for different ARM64 hardware configurations

## Documentation Style

### Code Comments
```bash
# ARM64-specific: Qt cross-compilation requires explicit toolchain
# This is necessary because Qt's build system doesn't auto-detect cross-compilers
if [[ $CARCH == "aarch64" ]]; then
    export QMAKESPEC=linux-aarch64-gnu-g++
    export PKG_CONFIG_PATH="/usr/lib/aarch64-linux-gnu/pkgconfig"
fi
```

### Documentation Patterns
- Always explain why ARM64-specific changes are necessary
- Include examples of both successful and failed compilation attempts
- Document hardware-specific considerations and limitations
- Provide troubleshooting steps for common cross-compilation errors

### README Conventions
- **Prerequisites section**: Clear ARM64 toolchain requirements
- **Build instructions**: Step-by-step cross-compilation guide
- **Testing procedures**: Validation steps for ARM64 binaries
- **Troubleshooting**: Common issues and solutions

### API Documentation
- Document all custom functions with parameter types and return values
- Include examples of PKGBUILD variables and their ARM64-specific values
- Explain integration points with other submodules
- Provide migration guides for x86_64 to ARM64 adaptations

## Security Considerations

### Security Best Practices
- Verify checksums for all source packages and patches
- Use GPG signature validation for critical components
- Implement proper input sanitization in build scripts
- Follow principle of least privilege for package installations

### Vulnerability Prevention
- Regular security updates for Qt/KDE frameworks
- Scan cross-compiled binaries for known vulnerabilities
- Validate all external dependencies for ARM64 compatibility
- Use secure defaults for installer configurations

### Secure Coding Patterns
```bash
# Input validation for package names
validate_package_name() {
    local pkg="$1"
    [[ "$pkg" =~ ^[a-zA-Z0-9._+-]+$ ]] || {
        echo "Error: Invalid package name '$pkg'" >&2
        return 1
    }
}

# Secure file operations
download_source() {
    local url="$1" checksum="$2"
    curl -fsSL "$url" -o source.tar.gz
    echo "$checksum source.tar.gz" | sha256sum -c || {
        echo "Error: Checksum verification failed" >&2
        return 1
    }
}
```

### Secrets Management
- Never include credentials or API keys in PKGBUILD files
- Use environment variables for sensitive build parameters
- Implement secure storage for package signing keys
- Validate GPG signatures on upstream source packages

## Performance Guidelines

### Optimization Priorities
1. **Cross-compilation Speed**: Optimize build parallelization and caching
2. **Package Size**: Minimize installed package footprint for ARM64 devices
3. **Runtime Performance**: ARM64-specific optimizations for target hardware
4. **Build Resource Usage**: Efficient use of memory and disk during compilation

### Performance Patterns
```bash
# Parallel compilation with resource limits
export MAKEFLAGS="-j$(nproc --ignore=1)"  # Leave 1 CPU for system
export CFLAGS="-march=armv8-a -mtune=cortex-a72 -O2"
export CXXFLAGS="$CFLAGS"

# Memory-efficient linking
export LDFLAGS="-Wl,--as-needed -Wl,--gc-sections"
```

### Resource Management
- Monitor disk space during cross-compilation (Qt builds can be large)
- Implement cleanup routines for intermediate build artifacts
- Use ccache for faster rebuild times during development
- Optimize Docker layer caching for containerized builds

### Profiling Approaches
- Time individual package builds to identify bottlenecks
- Profile memory usage during Qt/KDE compilation
- Analyze package size impact of different optimization levels
- Monitor runtime performance on target ARM64 hardware

## ARM64-Specific Considerations

### ARM64 Compatibility
```bash
# Compiler optimization for common ARM64 cores
case "${ARM64_TARGET:-generic}" in
    "rpi4"|"rpi5")
        export CFLAGS="-march=armv8-a -mtune=cortex-a72"
        ;;
    "pine64")
        export CFLAGS="-march=armv8-a -mtune=cortex-a53"
        ;;
    *)
        export CFLAGS="-march=armv8-a"
        ;;
esac
```

### Cross-Platform Concerns
- Build system compatibility between x86_64 host and ARM64 target
- Library path resolution for cross-compilation sysroot
- Endianness considerations (ARM64 is little-endian like x86_64)
- ABI compatibility between different ARM64 implementations

### Architecture-Specific Optimizations
- Use ARM64 NEON SIMD instructions where beneficial
- Optimize for ARM64 cache hierarchy and memory patterns
- Configure Qt for ARM64 graphics acceleration capabilities
- Tune package configurations for ARM64 hardware constraints

### Hardware Considerations
- Different ARM64 SoCs have varying capabilities (GPU, multimedia)
- Memory constraints on development boards vs. servers
- Storage limitations requiring size-optimized packages
- Network capabilities affecting download and update strategies

## Upstream Compatibility

### Compatibility Requirements
- Maintain 100% functional compatibility with x86_64 AcreetionOS packages
- Follow Arch Linux packaging standards and naming conventions
- Preserve AcreetionOS branding and customization elements
- Support standard Linux package management interfaces

### Integration Standards
```bash
# Standard desktop integration
[Desktop Entry]
Categories=System;Settings;Qt;KDE;
MimeType=application/x-desktop;
Keywords=installer;system;setup;
# ARM64-specific considerations documented in comments
```

### Contribution Guidelines
- Document all ARM64 modifications for potential upstream contribution
- Create separate branches for experimental ARM64 features
- Test backward compatibility with existing AcreetionOS configurations
- Provide clear migration paths for configuration updates

### Version Management
- Track upstream AcreetionOS package versions
- Document ARM64-specific version requirements and constraints
- Maintain compatibility matrices for Qt/KDE framework versions
- Implement version detection in build scripts for dependency validation

## Project-Specific Context

### AcreetionOS Integration
- Preserve all AcreetionOS branding and visual identity
- Maintain compatibility with AcreetionOS repository structure
- Support AcreetionOS-specific configuration and customization tools
- Ensure seamless integration with AcreetionOS system management

### Multi-Repository Coordination
- **arm64-toolchain**: Source cross-compilation environment and tools
- **package-builder**: Coordinate automated build workflows
- **iso-builder**: Provide compiled packages for ISO inclusion
- **hardware-support**: Integrate hardware-specific configurations

### Submodule Dependencies
- **Critical**: arm64-toolchain must be functional for any package building
- **Build Integration**: package-builder provides CI/CD automation
- **Distribution**: iso-builder includes packages in final ARM64 images
- **Testing**: testing-infrastructure provides validation environments

### Build System Integration
- Coordinate with package-builder for automated compilation workflows
- Integrate with releases submodule for package artifact distribution
- Sync with testing-infrastructure for validation and quality assurance
- Support upstream-sync for coordination with AcreetionOS upstream

## Common Patterns and Anti-Patterns

### Recommended Patterns
```bash
# Good: Conditional architecture handling
build() {
    if [[ $CARCH == "aarch64" ]]; then
        ./configure --host=aarch64-linux-gnu --enable-arm64-optimizations
    else
        ./configure --host=x86_64-linux-gnu
    fi
}

# Good: Proper cross-compilation environment
prepare() {
    if [[ -n "${CROSS_COMPILE:-}" ]]; then
        export CC="${CROSS_COMPILE}gcc"
        export CXX="${CROSS_COMPILE}g++"
        export PKG_CONFIG_PATH="/usr/lib/aarch64-linux-gnu/pkgconfig"
    fi
}
```

### Anti-Patterns to Avoid
```bash
# Bad: Hardcoded architecture assumptions
depends=('lib32-gcc-libs')  # x86_64-specific dependency

# Bad: No cross-compilation detection
gcc -march=x86-64  # Assumes x86_64 build environment

# Bad: Ignoring ARM64 ABI differences
# Assuming pointer size or endianness without checking
```

### Best Practices
- Always validate ARM64 toolchain availability before building
- Use architecture detection rather than hardcoded assumptions
- Implement comprehensive error handling for cross-compilation failures
- Document all hardware-specific requirements and limitations

### Code Review Focus
- ARM64 package functionality equivalent to x86_64 versions
- Cross-compilation correctness and reproducibility
- Integration compatibility with other AcreetionOS components
- Performance optimization for target ARM64 hardware platforms

## Error Handling and Logging

### Error Handling Strategy
```bash
# Comprehensive error handling for package building
build_package() {
    local pkg="$1"

    # Validate environment
    validate_cross_toolchain || return 1

    # Attempt build with detailed error reporting
    if ! makepkg -s --config build-configs/arm64.conf; then
        echo "Build failed for package: $pkg" >&2
        echo "Check build log: makepkg-$(date +%Y%m%d).log" >&2
        return 1
    fi

    # Validate ARM64 binary output
    validate_package_architecture "$pkg" || return 1
}
```

### Logging Conventions
- Use structured logging with timestamps and severity levels
- Log all cross-compilation environment setup and validation
- Include detailed package build information and timing
- Separate logs for different build phases and error types

### Debug Information
```bash
# Debug output for cross-compilation issues
debug_cross_compilation() {
    echo "=== Cross-compilation Environment ==="
    echo "TARGET_ARCH: ${TARGET_ARCH:-not set}"
    echo "CROSS_COMPILE: ${CROSS_COMPILE:-not set}"
    echo "CC: ${CC:-not set}"
    echo "CXX: ${CXX:-not set}"
    echo "PKG_CONFIG_PATH: ${PKG_CONFIG_PATH:-not set}"

    echo "=== Toolchain Validation ==="
    command -v "${CROSS_COMPILE}gcc" >/dev/null || echo "Cross-compiler not found"
    "${CROSS_COMPILE}gcc" --version 2>/dev/null || echo "Cross-compiler not functional"
}
```

### Monitoring Approaches
- Track build success rates for different packages
- Monitor build times and resource usage patterns
- Log package size and optimization effectiveness
- Implement health checks for cross-compilation environment

## Development Workflow

### Branch Management
- `feature/arm64-packagename` for individual package ARM64 adaptations
- `fix/build-packagename` for build system and compilation fixes
- `optimization/packagename-perf` for performance improvements
- `upstream/packagename-sync` for upstream compatibility updates

### Commit Conventions
```
feat(calamares): add ARM64 cross-compilation support
fix(lib2fix): resolve library linking issues for ARM64
perf(updater): optimize ARM64 binary size and performance
docs(calamares): document Qt cross-compilation requirements
```

### PR Guidelines
- Always test package builds on both x86_64 host and ARM64 target
- Include before/after package size comparisons for optimization changes
- Document any new dependencies or build requirements
- Verify functionality on actual ARM64 hardware when possible

### Review Process
- **Technical Review**: ARM64 compatibility and cross-compilation correctness
- **Functional Review**: Package functionality equivalent to x86_64 version
- **Performance Review**: Build time, package size, and runtime performance
- **Integration Review**: Compatibility with other AcreetionOS components

---

*These instructions help GitHub Copilot provide contextually appropriate suggestions for custom-packages development within the AcreetionOS ARM64 project ecosystem.*