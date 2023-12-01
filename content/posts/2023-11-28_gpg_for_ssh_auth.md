---
title: "GPG key for SSH authentication"
date: 2023-11-28
draft: false
tags: ["GPG", "Cryptography"]
---

Useful commands:

- `gpg --export-ssh-key <uid>`: Retrieve public key part of GPG/~~(used for)~~SSH key.
- `ssh-add --apple-use-keychain`
- `killall ssh-agent`
- `gpg --export --armor 3921648AF0ADA1F6`
- `gpg -K --keyid-format long --with-keygrip`
- `gpgconf --launch gpg-agent`: Launch gpg-agent manually.
- `ps aux | grep <gpg-agent|ssh-agent>`: Check if gpg-agent/ssh-agent is running.
- `set -x SSH_AUTH_SOCK $(gpgconf --list-dirs agent-ssh-socket)`: Tell SSH how to access gpg-agent by setting environment variable `SSH_AUTH_SOCK`.
- `ssh-add -L`: Get SSH key fingerprint.
- `gpgconf --kill gpg-agent`
- `ssh-agent -k`: Kill the current running ssh-agent.

---

Configure SSH to use GPG agent for authentication:

0. `echo $SSH_AUTH_SOCK`: Check current `SSH_AUTH_SOCK`, it may look like `/private/tmp/com.apple.launchd.BhS0P6FUJX/Listeners` on Mac.
1. `set -x SSH_AUTH_SOCK $(gpgconf --list-dirs agent-ssh-socket)`: Change that environment variable (`SSH_AUTH_SOCK`) to tell SSH how to access gpg-agent.
	- Rerun `echo $SSH_AUTH_SOCK`, it should output:
		- `/Users/thom/.gnupg/S.gpg-agent.ssh`
2. `gpgconf --launch gpg-agent`: Now, launch gpg-agent.
	- If it's not working (when we check by using `ssh -T git@github.com` failed), kill the current running gpg-agent: `gpgconf --kill gpg-agent`. Try `gpg-agent --daemon`/`gpgconf --launch gpg-agent` instead.
	- **OR** use reload agent command: `gpg-connect-agent reloadagent /bye`. ([Reference](https://wiki.archlinux.org/title/GnuPG#Reload_the_agent))
1. `ssh-add -L`: Use this command to check current identities. You can see it's activated.
	- It should output something like:
		- `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILPHzucXNBTMScAYA84tNcCs53L2WmveAVOZDuEP3x8p (none)`
		- This ⬆️ can be retrieved via `gpg --export-ssh-key <uid>`.

---

Retrieve SSH Key passphrase from Apple Keychain and add it to the ssh-agent for SSH authentication:

0. `ssh-add -L`: Check current ssh-agent authentication state.
1. `ssh-add --apple-use-keychain`: Add SSH private key to the ssh-agent using passphrase stored in the keychain.
2. `ssh -T git@github.com`: Test connection.
3. Done.

---

How to setup GPG for SSH authentication:

1. Edit `~/.gnupg/gpg-agent.conf` file, `echo "enable-ssh-support" >> ~/.gnupg/gpg-agent.conf`.
2. Create a subkey for authentication only, after creations, your keyring should look like this:

```
$ gpg -K --keyid-format long --with-keygrip
sec   rsa2048 2019-03-21 [SC] [expires: 2021-03-20]
      96F33EA7F4E0F7051D75FC208715AF32191DB135
      Keygrip = 90E08830BC1AAD225E657AD4FBE638B3D8E50C9E
uid           [ultimate] Brian Exelbierd
ssb   rsa2048 2019-03-21 [E] [expires: 2021-03-20]
      Keygrip = 5FA04ABEBFBC5089E50EDEB43198B4895BCA2136
ssb   rsa2048 2019-03-21 [A]
      Keygrip = 7710BA0643CC022B92544181FF2EAC2A290CDC0E
```

3. Take a note of the newly created subkey's keygrip, then: `cat 7710BA0643CC022B92544181FF2EAC2A290CDC0E >> ~/.gnupg/sshcontrol`.
4. Lastly, change the environment variable `SSH_AUTH_SOCK` to the socket GPG agent setup for SSH communication: `set -x SSH_AUTH_SOCK $(gpgconf --list-dirs agent-ssh-socket)`.

---

How to setup SSH Key for SSH authentication:

- [Connect with SSH | Checking for existing SSH keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

---

Knowledge:

> `$SSH_AUTH_SOCK` contains the path of the unix file socket that the agent uses for communication with other processes. This _is_ essential for `ssh-add`.

> `$SSH_AUTH_SOCK` : Identifies the path of a UNIX-domain socket used to communicate with the agent.

> When you run `ssh-add`, it communicates with the authentication agent (whether it's `ssh-agent` or `gpg-agent` acting as an SSH agent) via the specified socket, and that's what the `SSH_AUTH_SOCK` environment variable is used for.

> The `gpgconf --list-dirs agent-ssh-socket` command is used to display the directory where the GnuPG (GPG) agent's UNIX domain socket for SSH operations is located.

---

Reference:

- [How to enable SSH access using a GPG key for authentication](https://opensource.com/article/19/4/gpg-subkeys-ssh): Entry point for this topic/technique.
- [GPG - SSH setup](https://gist.github.com/mcattarinussi/834fc4b641ff4572018d0c665e5a94d3): more comprehensive.
- [SSH key - Arch Wiki](shttps://wiki.archlinux.org/title/SSH_keys)
- [GnuPG - Arch Wiki](https://wiki.archlinux.org/title/GnuPG)
- [OpenSSH - Arch Wiki](https://wiki.archlinux.org/title/OpenSSH)
