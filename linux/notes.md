# Linux General Notes

## Command chaining & job control

`;` simply ends that command so another command can follow (runs the next
command regardless of whether the previous one succeeded):
```bash
<command-1>; <command-2>
```

`&&` runs the next command only if the previous one succeeded (exit code 0):
```bash
<command-1> && <command-2>
```

`||` runs the next command only if the previous one failed (non-zero exit
code):
```bash
<command-1> || <command-2>
```

`&` runs that command in the background and immediately continues with the
next (or returns you to the prompt):
```bash
<command> &
```

`|` pipes the standard output of the left command into the standard input of
the right command:
```bash
<command-1> | <command-2>
```

Group commands so they share redirections or run in a subshell:
```bash
(<command-1>; <command-2>)
```
