# medic-bash

Bash helpers for writing medic scripts in bash. See [medic](https://github.com/synchronal/medic-rs)
for more info.

## Installation

```shell
brew install synchronal/tap/medic-bash
```

## Usage

This project provides a set of bash files that can be sourced in order to make scripts
to verify project setup and automate workflow.

```shell
#!/usr/bin/env bash

source $(brew --prefix)/share/medic-bash/doctor.bash
source $(brew --prefix)/share/medic-bash/confirm.bash

check "Direnv file exists" "test -e .envrc" "touch .envrc"
step "Run database migrations" "mix ecto.migrate"
confirm "Continue"

cecho -n --green "Output" --yellow "text" --cyan "to" --bright-bold-red "user"
```

### cecho

Colorized echo.

```shell
cecho -n --cyan "Cyan text" --green "OK"
```

### check

Run a check. If it returns a 0 exit status, then continue. If it returns a non-0 exit
status, then suggest a remedy and add it to the system clipboard (`pbcopy` or `xclip`).

Used in places where a dependency is required, and a specific remedy can be suggested,
but developers may choose to fulfill the dependency through some alternative mechanism.

```shell
check "Description" \
  "check command" \
  "remedy"
```

Examples: database must be running; language is installed.

### step

Run a step. If it returns a non-0 exit status, then stop everything.

Used in places where only one command will fulfill the dependency, and if it fails
there is no specific remedy.

```shell
step "Description" \
  "command"
```

Examples: git pull; install <mix|cargo|npm> dependencies; run database migrations.


## Usage with `medic`

Medic allows for shell actions to be performed. These may be shell scripts that internally use
`medic-bash`. 

```toml
[shipit]

steps = [
  { audit = {} },
  { update = {} },
  { test = {} },
  ## shell step using medic-bash
  { name = "Release", shell = "bin/dev/release", verbose = true },
  ##
  { step = "git", command = "push" },
  { step = "github", command = "link-to-actions", verbose = true },
]
```

```shell
#!/usr/bin/env bash

source $(brew --prefix)/share/medic-bash/doctor.bash

step "compile for release" "..."
step "create tar file" "..."
```
