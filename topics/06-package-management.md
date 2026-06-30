# 6. Package Management

Commands differ by distro family. Know both — IT Ops environments mix distros.

## Debian / Ubuntu (apt)

```bash
sudo apt update                 # refresh package index
sudo apt upgrade                  # upgrade installed packages
sudo apt install nginx              # install a package
sudo apt remove nginx                 # remove a package, keep config
sudo apt purge nginx                    # remove package + config files
apt search nginx                          # search for a package
apt show nginx                              # show package details
sudo apt autoremove                           # clean up unused dependencies
```

## RHEL / CentOS / Fedora (dnf / yum)

```bash
sudo dnf update                  # update all packages
sudo dnf install nginx             # install a package
sudo dnf remove nginx                # remove a package
dnf search nginx                       # search for a package
dnf info nginx                           # show package details
sudo dnf autoremove                        # remove unused dependencies
```

## Querying installed packages

```bash
dpkg -l                  # list installed packages (Debian-based)
dpkg -L nginx               # list files installed by a package
rpm -qa                        # list installed packages (RHEL-based)
rpm -ql nginx                     # list files from an RPM package
which nginx                          # path of an installed binary
```

## Adding repositories

```bash
sudo add-apt-repository ppa:something/ppa     # Debian/Ubuntu
sudo dnf config-manager --add-repo <url>          # RHEL/Fedora
```

## Common gotchas

- Always `apt update` before `apt install` on a fresh system — otherwise you're installing from a stale index.
- `apt remove` vs `apt purge`: purge also wipes config files — useful for a truly clean reinstall, dangerous if you wanted to keep settings.
- Mixing package managers (e.g. installing the same software via apt and manually via source) leads to version conflicts down the line.
