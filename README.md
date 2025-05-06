# JavaScript PNPM Build

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/js-pnpm-build?style=flat-square&label=Latest%20Release&color=blue)

## Description

A GitHub Action that builds JavaScript/TypeScript projects using PNPM and NX. This action is designed to work in conjunction with the [JavaScript PNPM Setup](https://github.com/p6m-actions/js-pnpm-setup) action, which handles Node.js installation, PNPM setup, and dependency caching.

## Usage

```yaml
- name: Setup PNPM
  uses: p6m-actions/js-pnpm-setup@v1
  with:
    node-version: '18'
    install-dependencies: 'true'

- name: Build
  uses: p6m-actions/js-pnpm-build@v1
  with:
    run-lint: 'true'
    archive-coverage: 'true'
```

## Inputs

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-lint` | Whether to run linting | `true` | No |
| `lint-args` | Arguments for the lint command | `--all --parallel=5` | No |
| `build-args` | Arguments for the build command | `--all --parallel=5 --prod --exclude docs` | No |
| `node-options` | Node options for build process | `--max_old_space_size=4096` | No |
| `archive-coverage` | Whether to archive code coverage results | `false` | No |
| `coverage-path` | Path to the coverage reports | `coverage` | No |

## Outputs

| Name | Description |
|------|-------------|
| `affected-apps` | List of affected applications |

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
        
      - name: Build and Lint
        uses: p6m-actions/js-pnpm-build@v1
```

### Full Example with Custom Options

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
          
      - name: Setup PNPM
        uses: p6m-actions/js-pnpm-setup@v1
        with:
          node-version: '20'
        
      - name: Build and Lint
        uses: p6m-actions/js-pnpm-build@v1
        with:
          run-lint: 'true'
          lint-args: '--all --parallel=3'
          build-args: '--all --parallel=3 --prod'
          node-options: '--max_old_space_size=8192'
          archive-coverage: 'true'
          coverage-path: 'reports/coverage'
          
      - name: Get Apps
        run: echo "Affected apps: ${{ steps.build.outputs.affected-apps }}"
```