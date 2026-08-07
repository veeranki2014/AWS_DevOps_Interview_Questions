```markdown
# Shell Scripting Interview Questions and Answers

## 1. What is shell scripting?

Shell scripting is the practice of placing shell commands, control structures, variables, and functions in a file to automate operating-system tasks.

## 2. What is a shebang?

A shebang declares the interpreter used to execute a script:

```bash
#!/usr/bin/env bash
```

It must be the first line of an executable script.

## 3. Why might `/usr/bin/env bash` be used instead of `/bin/bash`?

`/usr/bin/env bash` locates Bash through the current `PATH`, which can improve portability. `/bin/bash` selects an explicit interpreter path and can be more predictable in controlled environments.

## 4. How do you make a script executable?

Run:

```bash
chmod +x script.sh
./script.sh
```

The user also needs permission to access the script and its parent directories.

## 5. What is the difference between executing and sourcing a script?

Executing starts the script in another process. Sourcing runs it in the current shell:

```bash
source script.sh
. script.sh
```

Sourced changes to variables or the working directory affect the current shell.

## 6. How do you define and reference a variable?

```bash
name="production"
echo "$name"
```

Do not put spaces around the assignment operator.

## 7. Why should shell variables usually be quoted?

Quoting prevents unwanted word splitting and wildcard expansion:

```bash
rm -- "$target_file"
```

Unquoted variables can turn one intended argument into multiple unintended arguments.

## 8. What is command substitution?

Command substitution captures command output:

```bash
current_date=$(date +%F)
```

Trailing newline characters are removed from the result.

## 9. What is the difference between single and double quotes?

Single quotes preserve text literally. Double quotes allow variable expansion, command substitution, and selected escape processing.

```bash
echo '$HOME'
echo "$HOME"
```

## 10. What are positional parameters?

Positional parameters contain arguments supplied to a script or function:

- `$0`: Script name
- `$1` through `$9`: Individual arguments
- `${10}`: Arguments above nine
- `$#`: Argument count
- `"$@"`: All arguments as separate words
- `"$*"`: All arguments combined according to separator behavior

## 11. Why is `"$@"` generally preferred over `$*`?

`"$@"` preserves each original argument as a separate value, including arguments containing spaces. `$*` can combine arguments and lose their boundaries.

## 12. How do you read user input?

Use `read`:

```bash
read -r -p "Environment: " environment
```

`-r` prevents backslashes from being interpreted as escapes.

## 13. What is an exit status?

An exit status is an integer returned by a command. Zero conventionally means success; nonzero indicates failure.

The previous status is available as:

```bash
$?
```

## 14. How do you exit a script with an error?

Use:

```bash
exit 1
```

Choose documented nonzero codes when callers need to distinguish failure types.

## 15. What does `&&` do?

The command after `&&` runs only if the preceding command succeeds:

```bash
build_application && deploy_application
```

## 16. What does `||` do?

The command after `||` runs only if the preceding command fails:

```bash
start_service || report_failure
```

## 17. What does `set -e` do?

It makes the shell exit for many unhandled command failures. Its behavior has important exceptions in conditions, pipelines, command substitutions, and compound commands, so explicit error handling is still necessary.

## 18. What does `set -u` do?

It treats expansion of an unset variable as an error. Optional variables can use safe expansions such as:

```bash
value=${OPTIONAL_VALUE:-default}
```

## 19. What does `set -o pipefail` do?

Without `pipefail`, a pipeline’s status normally comes from its last command. With `pipefail`, the pipeline fails when an earlier command fails, subject to Bash’s documented status behavior.

## 20. What is a common strict-mode declaration?

A commonly used starting point is:

```bash
set -Eeuo pipefail
```

It improves failure visibility but does not replace deliberate validation, quoting, cleanup, and error handling.

## 21. How do you write an `if` statement?

```bash
if command; then
    echo "Success"
else
    echo "Failure"
fi
```

The condition is determined by the command’s exit status.

## 22. What is the difference between `[ ]` and `[[ ]]`?

`[ ]` invokes the POSIX-compatible test syntax. `[[ ]]` is a Bash conditional construct with safer parsing and features such as pattern and regular-expression matching.

## 23. How do you compare strings safely?

```bash
if [[ "$environment" == "production" ]]; then
    echo "Production selected"
fi
```

## 24. How do you compare numbers?

Use arithmetic comparison:

```bash
if (( count > 10 )); then
    echo "High count"
fi
```

Traditional test operators include `-eq`, `-ne`, `-lt`, `-le`, `-gt`, and `-ge`.

## 25. How do you test files and directories?

Examples include:

```bash
[[ -e "$path" ]]  # Exists
[[ -f "$path" ]]  # Regular file
[[ -d "$path" ]]  # Directory
[[ -r "$path" ]]  # Readable
[[ -w "$path" ]]  # Writable
[[ -x "$path" ]]  # Executable
[[ -s "$path" ]]  # Nonempty
```

## 26. How do you write a `case` statement?

```bash
case "$action" in
    start) start_service ;;
    stop) stop_service ;;
    restart) restart_service ;;
    *) echo "Unsupported action" >&2; exit 2 ;;
esac
```

It is useful for command-line modes and pattern matching.

## 27. How do you write a `for` loop?

```bash
for environment in dev test prod; do
    echo "$environment"
done
```

For file processing, avoid parsing `ls`; use globs, `find`, or null-delimited input.

## 28. How do you write a `while` loop?

```bash
while read -r line; do
    printf '%s\n' "$line"
done < input.txt
```

This reads the file without piping the loop into a subshell.

## 29. How do you define a function?

```bash
deploy() {
    local environment=$1
    printf 'Deploying to %s\n' "$environment"
}
```

Functions return an exit status rather than arbitrary string data.

## 30. What does `local` do?

Inside a Bash function, `local` limits a variable’s scope to that function and its called functions. It reduces accidental modification of global variables.

## 31. How do functions return data?

Use `return` for an exit status:

```bash
return 0
```

Return text through standard output and capture it with command substitution, or modify a deliberately scoped variable.

## 32. What is a trap?

A trap registers a command or function for a signal or shell event:

```bash
trap cleanup EXIT
```

It is commonly used for cleanup, termination handling, and error diagnostics.

## 33. How do you safely manage temporary files?

Use `mktemp` and register cleanup immediately:

```bash
temp_dir=$(mktemp -d)
trap 'rm -rf -- "$temp_dir"' EXIT
```

Validate destructive targets and keep temporary paths narrowly scoped.

## 34. How do you parse command-line options?

Use `getopts` for short POSIX-style options:

```bash
while getopts ":e:v" option; do
    case "$option" in
        e) environment=$OPTARG ;;
        v) verbose=true ;;
        *) exit 2 ;;
    esac
done
```

## 35. What are standard input, output, and error?

The standard file descriptors are:

- `0`: Standard input
- `1`: Standard output
- `2`: Standard error

Scripts should normally send results to stdout and diagnostics to stderr.

## 36. How do you redirect output?

Examples:

```bash
command > output.txt
command >> output.txt
command 2> error.txt
command > output.txt 2>&1
```

`>` truncates, while `>>` appends.

## 37. What is a pipe?

A pipe connects one command’s standard output to another command’s standard input:

```bash
producer | consumer
```

Pipeline stages may run in separate subshell environments.

## 38. What is a here-document?

A here-document provides multiline input:

```bash
cat <<EOF
Environment: $environment
Version: $version
EOF
```

Quote the delimiter, such as `<<'EOF'`, to prevent expansion.

## 39. What is a here-string?

A here-string sends one expanded string to a command’s standard input:

```bash
grep "ready" <<< "$status"
```

It is supported by Bash but is not part of basic POSIX shell syntax.

## 40. How should a script process filenames containing spaces or newlines?

Use null-delimited records:

```bash
find "$directory" -type f -print0 |
while IFS= read -r -d '' file; do
    process_file "$file"
done
```

Quote every filename expansion.

## 41. Why is parsing `ls` output unsafe?

`ls` formats output for humans, and filenames may contain spaces, tabs, newlines, or control characters. Use globs, `find`, or filesystem APIs instead.

## 42. How do you perform arithmetic?

Use arithmetic expansion or evaluation:

```bash
total=$((price * quantity))
((counter++))
```

Bash arithmetic is integer-based unless an external tool is used.

## 43. How do arrays work in Bash?

Indexed array:

```bash
servers=("app-1" "app-2")
printf '%s\n' "${servers[@]}"
```

Associative array:

```bash
declare -A ports=([http]=80 [https]=443)
```

## 44. How do you debug a shell script?

Use syntax validation and tracing:

```bash
bash -n script.sh
bash -x script.sh
```

Also add structured logs, inspect exit statuses, and use ShellCheck for static analysis.

## 45. What is ShellCheck?

ShellCheck is a static-analysis tool for shell scripts. It detects common quoting, portability, expansion, redirection, and control-flow errors.

## 46. How do you prevent multiple script instances from running simultaneously?

Use an atomic lock mechanism such as `flock`:

```bash
flock -n /run/lock/job.lock command
```

A plain “lock file exists” check can suffer from races and stale files.

## 47. How do you implement retries?

Use a bounded loop with delay and clear failure handling:

```bash
for attempt in 1 2 3; do
    if perform_operation; then
        break
    fi

    if (( attempt == 3 )); then
        echo "Operation failed" >&2
        exit 1
    fi

    sleep $((attempt * 5))
done
```

Retry only operations that are safe and likely to fail transiently.

## 48. How would you check whether a service is running?

For a systemd service:

```bash
if systemctl is-active --quiet nginx; then
    echo "nginx is running"
else
    echo "nginx is not running" >&2
    exit 1
fi
```

Application health may require an endpoint check rather than process status alone.

## 49. How should logs be written from a script?

Include timestamps, severity, operation context, and clear errors. Avoid logging secrets:

```bash
log() {
    printf '%s [%s] %s\n' \
        "$(date -Is)" "$1" "$2" >&2
}
```

## 50. What makes a production-quality shell script?

A production-quality script uses explicit inputs, validation, safe quoting, meaningful exit codes, controlled permissions, bounded retries, idempotent behavior, cleanup traps, structured logs, secret protection, ShellCheck, and automated tests.

