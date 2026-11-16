---
sidebar_position: 3
---

# Command Reference

[← Back to README](../README.md)

## Additions

| Scope | Command / Option | Abbreviation / Options | Meaning | Uses |
|---|---|---|---|---|
| Keys | `ssh-keygen -t ed25519 -f <path> -C "<comment>"` | `-t` = Type, `-f` = File, `-C` = Comment | Generate a new key pair | Create a key for server or GitHub |
| Keys | `ssh-keygen -y -f <key>` | `-y` = Output public key | Derive public key from private key | Check if a key has a passphrase |
| GitHub | `ssh -T git@github.com` | `-T` = no terminal request | Test connection to GitHub | Check if the key is accepted |
| GitHub | `git remote set-url origin git@github.com:<user>/<repo>.git` | - | Switch remote URL to SSH | Switch HTTPS clone to SSH |
| System | `sudo apt update && sudo apt upgrade -y` | `-y` = answer all prompts with yes | Update package lists and packages | Regular updates |
| System | `lsb_release -a` | `-a` = all information | Show Ubuntu version | Check version |
| Nginx | ​​`sudo nginx -t` | `-t` = test | Check configuration | Before every reload |
|Nginx | ​​`sudo nginx -T` | `-T` = Test + output | Show entire configuration | Search for `listen`/`server_name` |
|Nginx | ​​`sudo ss -tlnp` | `-t` TCP, `-l` listen, `-n` numeric, `-p` process | Show open ports | Which port is Nginx running on? |
|Nginx | ​​`sudo ln -s <source> <destination>` | `-s` = symbolic link | Create link | Enable site in `sites-enabled` |
|Firewall | `sudo ufw allow <port/service>` | - | Open port | Allow SSH/HTTP/HTTPS |
|Firewall | `sudo ufw status verbose` | - | Show firewall rules | Check what's open |
|Firewall| `sudo ufw default allow outgoing| -| allow all outgoing traffic| apt, git pull etc.|
|Firewall| `sudo ufw allow OpenSSH` | - | Open SSH port 22 | **Always first**, otherwise lockout |
| Firewall | `sudo ufw allow <port>/tcp` | `tcp` = protocol | Open a specific port | e.g., 80, 443, 8081 |
| Firewall | `sudo ufw enable` | - | Activate firewall | After all needed ports are open |
