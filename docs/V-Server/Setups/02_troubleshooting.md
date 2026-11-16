---
sidebar_position: 2
---

# Troubleshooting

[← Zurück zur README](../README.md)

## Table of content

| Message / Symptom | Probable Cause | Solution |
:--|:--|:--|
`Permission denied (publickey)` during server login | Incorrect username (case-sensitive) | [solution](#permission-denied-during-server-login) |
`Permission denied (publickey)` at GitHub | Key not offered | [solution](#permission-denied-at-github) |
`File ... is being edited by root ... open anyway?` | Nano-lock file after connection failure | [solution](#nano-lock-file) |
| Page not accessible in browser | Nginx stopped, configuration faulty, or port 

---

## Permission denied during server login

The server is rejecting the key. The local `~/.ssh/config` only determines **which** key is offered.

1. **Check the username.** Linux is case-sensitive:

```
id <user>
```
2. **Is a key stored?**
```
cat ~/.ssh/authorized_keys
```
3. **Diagnosis** on the client and in the server log:

```
ssh -v V-Server
sudo journalctl -u ssh -n 50
```




## Permission denied at GitHub

```
ssh -vT git@github.com 2>&1 | grep -Ei 'offering|identity file|denied'
```

- `type -1` after all identity files means: No default file exists, **no** key is offered. → Create `~/.ssh/config` with `IdentityFile` (see [step 9](../Setups/01_setup-steps-v-server.md#9-server-mit-github-verbinden)).

- Key is offered but rejected → Compare fingerprint with GitHub:
```
ssh-keygen -lf ~/.ssh/gitconnect.pub
```


## nano lock file

After a connection is lost, the lock file remains, even after a restart.

```
ps -p <PID> # no output = process no longer exists

ls -la /etc/ssh/ | grep -E 'swp|save'

sudo rm /etc/ssh/.sshd_config.swp
```

An existing `sshd_config.save` file contains **unsaved changes**. Review it first, then delete it.



