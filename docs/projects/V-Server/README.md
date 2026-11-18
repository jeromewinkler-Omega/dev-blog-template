---
sidebar_position: 4
---

# V-Server 
## Secure setup with SSH and Nginx

This repository documents the setup and security of a self-managed virtual server (Ubuntu). The focus is on a **secure basic configuration**: login only via SSH key, disabled password and root login, an Nginx web server with its own website, and connecting the server to GitHub.

## Table of content

- [Prerequisites](#Prerequisites)
- [Quickstart](#quickstart)
- [Usage – basic knowledge](#usage---basic-knowledge)
- [Additional Information](#additional-information)
- [Project Checklist](../../../static/files/Git%20+%20VServer%20Checkliste.pdf)

---

## Prerequisites

Tool | What for | Checking with | Obligation? 
|:--|:--|:--|:--:|
| **Git** | Clone the repository and push changes | `git --version` | x |
| **OpenSSH-Client** | Connection to the server (included in Windows 10/11) | `ssh -V` | x |
| **Access to the V-Server** | Username, public IP, initial password from the provider | `ssh <User>@<server-IP>` | x |
| **GitHub-Account** | Repository access, deployment key for the server | ssh -T git@github.com` | x |
| **Editor** (z. B. VS Code) | Edit files | – | recommended

 **Note:** This documentation uses placeholders such as `<user>`, `<server-ip>`, and `<key-name>`. Real IP addresses, usernames, and keys **do not** belong in a repository.

---

## Quickstart

The complete commands ar listet in  [setup/01-setup-schritte-v-server.md](setup/01-Setup-Schritte V-Server.md). These steps should be carried out in the specified order.

1. **clone repository** and open loacaly -> [step 1](../V-Server/Setups/01_setup-steps-v-server.md#1-clone-the-repository-on-pc)
2. **create SSH-Key-pair** (ed25519, with passphrase) -> [step 2](../V-Server/Setups/01_setup-steps-v-server.md#2-generate-an-ssh-key-pair)
3. **Establish First connection** to the server with password -> [step 3](../V-Server/Setups/01_setup-steps-v-server.md#3-first-connection-to-the-server)
4. **Add public key to authorized_keys** and try/test login with ssh key-pair -> [step 4](../V-Server/Setups/01_setup-steps-v-server.md#4-public-key-auf-dem-server-hinterlegen)
5. **Create an SSH configuration** so that ssh v-server is sufficient -> [step 5](../V-Server/Setups/01_setup-steps-v-server.md#5-create-ssh-config)
6. **Create safe login**: disable password login -> [step 6](../V-Server/Setups/01_setup-steps-v-server.md#6-harden-the-server)
7. **System update** and Nginx dependencie install -> [step 7](../V-Server/Setups/01_setup-steps-v-server.md#7-update-the-system-and-install-nginx)
8. **Create own page** deliver via an Nginx server block on port 8081 → [step 8](../V-Server/Setups/01_setup-steps-v-server.md#8-create-your-own-website-on-port-8081)
9. **Set config global user info**  -> [step 9](../V-Server/Setups/01_setup-steps-v-server.md#9-set-config-global-user-info)
10. **Connect server with GitHub** (Deploy Key) -> [step 10](../V-Server/Setups/01_setup-steps-v-server.md#10-server-mit-github-verbinden)
11. **Clone git repository** -> [step 11](../V-Server/Setups/01_setup-steps-v-server.md#11-clone-git-repository)
12. **Set Firewall** -> [Step 12](../V-Server/Setups/01_setup-steps-v-server.md#12-set-firewall)


If something doesn't go as expected:: [Troubleshooting](../V-Server/Setups/02_troubleshooting.md)

---

## Usage - basic knowledge

### what is a V-Server?

A VPS (Virtual Private Server) is a virtual machine in a data center. A Virtual machine management software divides a physical server into multiple isolated instances, each with a fixed allocation of CPU, RAM, and storage.

- Own operating system with full administrator rights

- Static, public IP address, meaning it's accessible from the internet 24/7

- Updates and security are your responsibility

Public or private? Public servers are constantly scanned by bots for open SSH ports, which is why the hardening steps in 6 are mandatory.

### SSH-key-pair

| File | Name | Where is it located? | Share? |
|:--|:--|:--|:--|
| `<key-name>` | Private Key |only on your local mashine |  never |
| `<key-name>.pub` | Public Key | on the server in `~/.ssh/authorized_keys`, on GitHub | yes / with GitHub  |

During login, your computer uses the private key to prove that it matches the stored public key. A password is never transmitted in this process

Passphrase vs. User Password: The passphrase only encrypts the private key file on your computer. The server knows nothing about it. `PasswordAuthentication no` therefore only disables the user's password; the passphrase is still requested locally. 

### Client and server configuration

| File | Where? | What for? |
|:--|:--|:--|
| `~/.ssh/config` | Client | How you connect to other servers  |
| `/etc/ssh/ssh_config` | Client (system-wide) | Default values ​​for outgoing connections |
| `/etc/ssh/sshd_config` | Server | Who is allowed to log in to this server and how |



### Server-hardening

| Option in `sshd_config` | Recommended | Why |
|:--|:--|:--|
`PubkeyAuthentication` | `yes` | Allow login using a key |
`PasswordAuthentication` | `no` | Passwords can be guessed, keys not as easy |
`KbdInteractiveAuthentication` | `no` | Disables the second password method via PAM |


:::warning
**Golden Rule:** Before making any changes to the SSH configuration, leave a session open and check in a **second** window whether the login still works.
:::

### Nginx

Nginx is a web server. A server block specifies which port Nginx listens on and from which directory it serves files.

- On Debian/Ubuntu, the configurations are located in `/etc/nginx/sites-available/` and are enabled via link in `sites-enabled/`.


### GitHub connect from Server

A server key without a passphrase is common for automated processes. Store it as a deployment key without write access to limit potential damage.

All commands with explanation: [Commands](../V-Server/Setups/03_commands.md)

---

## Additional Information

### Security information

- Never commit private keys, passwords, or real server IPs. A `.gitignore` file containing `*.pem`, `id_*`, and `.env` will help.

- Before making major changes (e.g., `do-release-upgrade`) with the provider, take a **snapshot**.

- If you are locked out: The VPS provider's **web/VNC console** works even without SSH.

- If the connection is unstable, work in `tmux`. After an interruption, continue with `tmux attach`.


### Links

- [Nginx Dokumentation](https://nginx.org/en/docs/)
- [GitHub – Mit SSH verbinden](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

 

