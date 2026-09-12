# Linux Quick Commands

## Search Files & Text

Search a directory tree recursively for one of several patterns in file
contents (case-insensitive, line numbers, extended regex) — e.g. hunting for
JVM/proxy config left in a repo that could break CI on a different machine
(patterns like `java.security`, `JAVA_TOOL_OPTIONS`, `MAVEN_OPTS`, `JDK_JAVA_OPTIONS`):
```bash
grep -rniE "<pattern-1>|<pattern-2>|<pattern-3>" .
```

Find files matching any of several name patterns across a directory tree —
e.g. locating config files that shouldn't carry machine-local paths
(patterns like `*.mvn*`, `jvm.config`, `maven.config`):
```bash
find . -iname "<name-pattern-1>" -o -iname "<name-pattern-2>" -o -iname "<name-pattern-3>"
```

Search recursively and list only the matching file paths (no content, no
line numbers) — case-insensitive:
```bash
grep -irl "text to search" .
```

Find files (not directories) by a case-insensitive name pattern:
```bash
find . -type f -iname "*filename*"
```

Search for text within a single file, showing line numbers:
```bash
grep -n "text to search" <file>
```

## Navigation & Files

Show the current working directory:
```bash
pwd
```

List all files (including hidden) with details:
```bash
ls -la
```

Jump back to the previous directory:
```bash
cd -
```

Create a directory, including any missing parent directories:
```bash
mkdir -p <path>
```

Copy a directory recursively:
```bash
cp -r <source> <destination>
```

Remove a directory and its contents (careful — no undo):
```bash
rm -rf <path>
```

List the N most recently modified regular files in the current directory
(excludes subdirectories):
```bash
ls -lt | grep "^-" | head -n <N>
```

## Permissions & Ownership

Make a file executable:
```bash
chmod +x <file>
```

Change the owner (and group) of a file/directory:
```bash
sudo chown <user>:<group> <path>
```

## Disk & Resource Usage

Show disk space usage per mounted filesystem, human-readable:
```bash
df -h
```

Show the total size of a directory, human-readable:
```bash
du -sh <path>
```

Show free/used memory, human-readable:
```bash
free -h
```

Show running processes sorted by CPU/memory usage (interactive):
```bash
top
```

## Process Management

Find the process(es) matching a name:
```bash
ps aux | grep <name>
```

Kill a process by PID:
```bash
kill -9 <pid>
```

Kill all processes matching a name:
```bash
killall <name>
```

Run a command in the background, immune to hangups (keeps running after you
log out or close the terminal):
```bash
nohup <command> &
```

## Networking

Show listening ports and the processes bound to them:
```bash
sudo ss -tulpn
```

Download a file from a URL:
```bash
curl -O <url>
```

Copy a file to/from a remote host over SSH:
```bash
scp <local-path> <user>@<host>:<remote-path>
```

## Archives

Create a gzip-compressed tarball:
```bash
tar -czvf <archive-name>.tar.gz <path>
```

Extract a gzip-compressed tarball:
```bash
tar -xzvf <archive-name>.tar.gz
```

## Text Processing

View a large file page by page (search with `/`, quit with `q`):
```bash
less <file>
```

Follow a file's new content live (e.g. a log being written to):
```bash
tail -f <file>
```

Count lines in a file:
```bash
wc -l <file>
```

Count occurrences of each unique line (e.g. top values in a column):
```bash
sort <file> | uniq -c | sort -rn
```

Decode a Base64 string to plaintext (`-d` is GNU/Linux; on macOS use `base64 -D` or `base64 --decode`):
```bash
echo "<base64-string>" | base64 -d
```
