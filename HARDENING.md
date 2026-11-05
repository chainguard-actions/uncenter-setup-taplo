<!-- markdownlint-disable -->

# Hardening Report: uncenter--setup-taplo/v1.0.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uncenter--setup-taplo/v1.0.7** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) is violated: the 'Install Taplo' step puts GitHub Actions expressions directly into its run: script. The offending lines are `case "${{ runner.os }}" in` (line 17), `case "${{ runner.arch }}" in` (line 30) and `curl -Ls "https://github.com/tamasfe/taplo/releases/download/${{ inputs.version }}/taplo-full-${RUNNER_OS}-${RUNNER_ARCH}.gz"` (line 42). inputs.version is controlled by whoever calls the action, so it can inject shell commands. To fix this, move the values into env: variables (e.g. TAPLO_VERSION: ${{ inputs.version }}) and use double-quoted shell variables such as "$RUNNER_OS", "$RUNNER_ARCH" and "${TAPLO_VERSION}".

Locations:

- `action.yml:17`
- `action.yml:30`
- `action.yml:42`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.version }}" appears directly in run: block of step "Install Taplo"; move to env: map

Locations:

- `action.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

In action.yml, the 'Install Taplo' step's run: script no longer contains ${{ runner.os }}, ${{ runner.arch }} or ${{ inputs.version }}. They are now set in the step's env: block as TAPLO_RUNNER_OS, TAPLO_RUNNER_ARCH and TAPLO_VERSION, and the script reads them as double-quoted shell variables. I used names with a TAPLO_ prefix on purpose: the script already reassigns RUNNER_OS and RUNNER_ARCH to lowercase download-name values, so reusing those names would mix the two meanings. Nothing else in the step changed. Not tested by running the action.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

In action.yml, replaced case "${{ runner.os }}" with case "$TAPLO_RUNNER_OS". TAPLO_RUNNER_OS was already defined in the step's env: block. The run script now contains no ${{ }} expressions.

