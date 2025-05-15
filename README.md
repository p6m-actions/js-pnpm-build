# JavaScript PNPM Build

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/js-pnpm-build?style=flat-square&label=Latest%20Release&color=blue)

## Description

A GitHub Action that builds JavaScript/TypeScript projects using pnpm. This action provides flexible configuration for linting, testing, and building with sensible defaults and clear error reporting. It's designed to work with any pnpm-based project including Vue.js, Svelte/SvelteKit, Next.js, and other JavaScript/TypeScript frameworks.

This action is designed to work in conjunction with the [JavaScript PNPM Setup](https://github.com/p6m-actions/js-pnpm-setup) action, which handles Node.js installation, PNPM setup, and dependency caching.

## Features

- **Flexible Configuration**: Customize lint, test, and build commands with your own scripts
- **Sensible Defaults**: Works out-of-the-box with standard pnpm projects
- **Toggleable Steps**: Enable or disable linting, testing, and building steps as needed
- **Clear Error Reporting**: Each step provides clear feedback about failures
- **Coverage Reports**: Option to archive test coverage reports

## Usage

Basic usage with default settings:

```yaml
- name: Setup PNPM
  uses: p6m-actions/js-pnpm-setup@v1
  with:
    node-version: '18'
    install-dependencies: 'true'

- name: Build
  uses: p6m-actions/js-pnpm-build@v1
```

Customized usage with specific commands:

```yaml
- name: Build
  uses: p6m-actions/js-pnpm-build@v1
  with:
    # Linting configuration
    run-lint: 'true'
    lint-command: 'pnpm lint:strict'
    lint-args: '--fix'
    
    # Testing configuration
    run-test: 'true'
    test-command: 'pnpm test:coverage'
    
    # Build configuration
    run-build: 'true'
    build-command: 'pnpm build:prod'
    
    # Archive coverage reports
    archive-coverage: 'true'
```

## Inputs

### Lint Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-lint` | Whether to run linting | `true` | No |
| `lint-command` | Command to run for linting | `pnpm lint` | No |
| `lint-args` | Additional arguments for the lint command | ` ` | No |

### Test Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-test` | Whether to run tests | `true` | No |
| `test-command` | Command to run for testing | `pnpm test` | No |
| `test-args` | Additional arguments for the test command | ` ` | No |

### Build Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-build` | Whether to run build | `true` | No |
| `build-command` | Command to run for building | `pnpm build` | No |
| `build-args` | Additional arguments for the build command | ` ` | No |

### General Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `node-options` | Node options for all processes | `--max_old_space_size=4096` | No |
| `archive-coverage` | Whether to archive code coverage results | `false` | No |
| `coverage-path` | Path to the coverage reports | `coverage` | No |

## Examples

### Basic Usage

```yaml
name: Build and Test

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
        
      - name: Setup PNPM
        uses: p6m-actions/js-pnpm-setup@v1
        
      - name: Build and Test
        uses: p6m-actions/js-pnpm-build@v1
```

### Only Run Linting

```yaml
- name: Lint Only
  uses: p6m-actions/js-pnpm-build@v1
  with:
    run-lint: 'true'
    run-test: 'false' 
    run-build: 'false'
```

### Vue.js Project Example

```yaml
- name: Build Vue.js Project
  uses: p6m-actions/js-pnpm-build@v1
  with:
    lint-command: 'pnpm lint:vue'
    test-command: 'pnpm test:unit'
    build-command: 'pnpm build:prod'
```

### Next.js Project Example

```yaml
- name: Build Next.js Project
  uses: p6m-actions/js-pnpm-build@v1
  with:
    lint-command: 'pnpm lint'
    test-command: 'pnpm test:ci'
    build-command: 'pnpm build'
    node-options: '--max_old_space_size=8192' # Next.js may need more memory
```

### Workspace Project Example

```yaml
- name: Build Workspace Project
  uses: p6m-actions/js-pnpm-build@v1
  with:
    lint-command: 'pnpm -r lint'
    test-command: 'pnpm -r test'
    build-command: 'pnpm -r build'
```

## Troubleshooting

### Common Issues

1. **Command not found errors**: Make sure your package.json includes the scripts defined in your lint-command, test-command, and build-command inputs.

2. **Out of memory errors**: If you encounter memory issues during build, increase the memory allocation with the node-options input.

3. **pnpm workspace issues**: For monorepo projects using pnpm workspaces, you may need to use commands like `pnpm -r` to run commands recursively in all packages.