# curl Quick Commands

Use this reference for common HTTP requests, downloads, and installer scripts.

## Basics

Check the installed curl version:
```bash
curl --version
```

Show curl's command-line help:
```bash
curl --help
```

Make a basic GET request and print the response body:
```bash
curl <url>
```

Show the response headers along with the response body:
```bash
curl -i <url>
```

## Downloading Files

Download a URL to a local file, following redirects:
```bash
curl -L <url> -o <file-path>
```

## Running Installer Scripts

Run an installer script directly from a trusted repository. Replace `<github-username>` and `<repository-name>` with the GitHub account and repository containing `install.sh`; review the script and trust its source before running it:
```bash
curl -fsSL https://raw.githubusercontent.com/<github-username>/<repository-name>/main/install.sh | bash
```

Download the installer script from a trusted repository for inspection before running it:
```bash
curl -fsSL https://raw.githubusercontent.com/<github-username>/<repository-name>/main/install.sh -o install.sh
```

Make the downloaded installer executable:
```bash
chmod +x install.sh
```

Run the installer:
```bash
./install.sh
```
