# Medic Bash

Bash helpers for writing medic scripts in bash.

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

