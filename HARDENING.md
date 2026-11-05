<!-- markdownlint-disable -->

# Hardening Report: uncenter--setup-taplo/v2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uncenter--setup-taplo/v2** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): the 'Install Taplo' step puts GitHub expressions straight into its run: script: `case "${{ runner.os }}" in` (line 17), `case "${{ runner.arch }}" in` (line 35), and `curl -Ls "https://github.com/tamasfe/taplo/releases/download/${{ inputs.version }}/taplo-${RUNNER_OS}-${RUNNER_ARCH}.gz"` (line 47). The caller controls inputs.version, so a crafted value can inject shell commands. Fix: pass these values in through env: variables (e.g. TAPLO_VERSION) or use the built-in $RUNNER_OS/$RUNNER_ARCH, and reference them double-quoted.

Locations:

- `action.yml:17`
- `action.yml:35`
- `action.yml:47`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.version }}" appears directly in run: block of step "Install Taplo"; move to env: map

Locations:

- `action.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

In action.yml, inputs.version, runner.os and runner.arch now reach the 'Install Taplo' step through its env: block as TAPLO_VERSION, TAPLO_RUNNER_OS and TAPLO_RUNNER_ARCH. The script reads them as double-quoted shell variables. I didn't reuse the names RUNNER_OS/RUNNER_ARCH because the script overwrites those variables with lowercase values (for example 'linux', 'x86_64') to build the download URL. I haven't run the action after the change.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Added env block with TAPLO_VERSION (inputs.version), TAPLO_RUNNER_OS (runner.os), TAPLO_RUNNER_ARCH (runner.arch); case statement now reads "$TAPLO_RUNNER_OS" so it doesn't conflict with the script's own RUNNER_OS assignment.

