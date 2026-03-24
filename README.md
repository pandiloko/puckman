# puckman

Installs software directly from GitHub repos and release artifacts. The goal is a simple "install from upstream releases" workflow, especially for fresh versions that may not be available in distro repositories yet.

Installs software directly from github repos. I wanted an approach to what Arch and AUR provide: cutting edge versions of software fresh from the repo oven.

The name `puckman` comes after the `pacman` command from Arch and is the original internal name of Pac-Man, the videogame character (a hockey-puck-like shape).

## Current features

- Resolves packages from a configured list (`packages.txt`) or lets you search GitHub.
- It smartly identifies the appropriate installer from the list of release artifact (don't get carried away, this intelligent detection is just a bunch of 'ifs'). 
- Fallback to manual selection whenever the detection fails, but saves the selection and uses it for future updates comparing using levenshtein algorithm
- Supports install paths for `.deb`, archives (`zip`, `tar.*`, `bz2`), and executable/script artifacts.
- Supports pip-based installs when a package entry is configured with a pip command.
- Records install manifests (v3.0+) under `PUCKMAN_STATE` (default `/var/lib/puckman/installed/`) so uninstalls are reversible.
- Provides dependency bootstrap via `requirements` action with interactive confirmation.

## Usage

```bash
puckman <action> [package]
```

Actions:

- `ls` / `list`
  - List available packages (and cached descriptions/stars).
- `install <package>`
  - Install package from list or direct owner/repo match.
- `uninstall <package>` (aliases: `remove`, `delete`)
  - Remove installs recorded by manifests.
- `search <term>` (aliases: `query`, `find`)
  - Search GitHub repos, select one, then install.
- `update` (alias: `upgrade`)
  - Update puckman itself (git pull or remote script replacement).
- `requirements` (aliases: `deps`, `install-deps`)
  - Install required and recommended runtime dependencies after confirmation.
- `<package>`
  - No explicit action: treated as `install <package>`.

## Dependency bootstrap

You can install all required + recommended packages with:

```bash
sudo apt update && sudo apt install -y file jq wget python3 sudo coreutils bzip2 unzip bsdextrautils python3-pylev
```

Or run:

```bash
sudo puckman requirements
```

`python3-pylev` is recommended (faster artifact matching) but not strictly required.

## Uninstall behavior (manifest-based)

Each successful install writes a JSON manifest in `PUCKMAN_STATE`. `puckman uninstall <name>` only removes what was recorded:

- `.deb`: purged with `apt-get purge`
- file installs: only tracked paths under `/usr/local/bin`
- pip installs: uses the same pip command that was recorded

Older/manual installs are not tracked and are not removed automatically.
