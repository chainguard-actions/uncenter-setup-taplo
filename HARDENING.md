<!-- markdownlint-disable -->

# Hardening Report: uncenter--setup-taplo/v1.0.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uncenter--setup-taplo/v1.0.8** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a), direct expression interpolation in the 'Install Taplo' run: block. Three lines are affected: `case "${{ runner.os }}" in` (line 17), `case "${{ runner.arch }}" in` (line 35), and `curl -Ls "https://github.com/tamasfe/taplo/releases/download/${{ inputs.version }}/taplo-full-${RUNNER_OS}-${RUNNER_ARCH}.gz"` (line 47). The inputs.version value can be set by the caller and is pasted into the script text before the shell runs it. Fix: pass the version through env (for example VERSION: ${{ inputs.version }}) and reference it as "$VERSION". Use the runner's own "$RUNNER_OS" and "$RUNNER_ARCH" env vars, and rename the script's local variables so they don't overwrite them.

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

Moved inputs.version to env TAPLO_VERSION; replaced ${{ runner.os }}/${{ runner.arch }} with runner-provided $RUNNER_OS/$RUNNER_ARCH; renamed script locals to TAPLO_OS/TAPLO_ARCH so they no longer overwrite runner env vars. Behavior otherwise unchanged.

