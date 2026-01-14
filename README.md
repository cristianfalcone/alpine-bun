# Alpine Linux APK Packages for Bun

Unofficial Alpine Linux packages for [Bun](https://bun.sh) - the incredibly fast JavaScript runtime, bundler, test runner, and package manager.

## Packages

| Package | Description | Dependencies | Size |
|---------|-------------|--------------|------|
| `bun-bootstrap` | Bootstrap binary for building from source | libstdc++, libgcc | 35.9 MB |
| `bun` | Bun runtime (dynamically linked) | libstdc++, libgcc | 35.1 MB |
| `bun-static` | Bun runtime (statically linked) | none | 35.4 MB |

## Installation

### 1. Add the signing key

```bash
wget -O /etc/apk/keys/builder-6967edfa.rsa.pub \
  https://raw.githubusercontent.com/cristianfalcone/alpine-bun/main/.keys/builder-6967edfa.rsa.pub
```

### 2. Add the repository

```bash
echo "https://raw.githubusercontent.com/cristianfalcone/alpine-bun/main/edge/testing" \
  >> /etc/apk/repositories
```

### 3. Install Bun

```bash
apk update

# Install dynamic version (recommended for most use cases)
apk add bun

# Or install static version (for minimal containers)
apk add bun-static

# Verify installation
bun --version
```

## Usage

```bash
# Check version
bun --version

# Run JavaScript
echo 'console.log("Hello from Bun!")' | bun run -

# Run a script
bun run script.ts

# Install packages
bun install

# Start HTTP server
bun eval "Bun.serve({port: 3000, fetch: () => new Response('OK')})"
```

## Building from Source

These APKBUILDs are designed to build Bun from source on Alpine Linux edge.

### Prerequisites

```bash
# On Alpine edge
apk add alpine-sdk atools
abuild-keygen -a -i
```

### Build Process

```bash
# Clone this repo
git clone https://github.com/cristianfalcone/alpine-bun.git
cd alpine-bun/aports

# Build bun-bootstrap first (required for bootstrapping)
cd bun-bootstrap
abuild -r

# Build bun (creates both bun and bun-static packages)
cd ../bun
abuild -r
```

## Package Details

### bun-bootstrap

Downloads the official musl binary from upstream to bootstrap the build process. This is necessary because Bun requires Bun to compile (similar to how Go and Rust handle bootstrapping in Alpine).

### bun (dynamic)

The main package, dynamically linked against libstdc++ and libgcc. This is the recommended version for most use cases.

### bun-static

A fully statically linked version with no shared library dependencies (except musl loader). Ideal for:
- Minimal container images
- Environments where libstdc++ is not available
- Embedded systems

## Contributing

This is a staging repository for testing before submitting to [Alpine aports](https://gitlab.alpinelinux.org/alpine/aports).

Related: [Package request #13998](https://gitlab.alpinelinux.org/alpine/aports/-/issues/13998)

## License

The APKBUILDs in this repository are provided under MIT license.
Bun itself is licensed under MIT - see [Bun License](https://github.com/oven-sh/bun/blob/main/LICENSE).
