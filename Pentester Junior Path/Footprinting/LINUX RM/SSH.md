Secure Shell is encrypted protocol for remote login, file transfer, and port forwarding. Standard port is TCP 22.
## Key Points

- Two protocols: SSH-1 (obsolete, insecure) and SSH-2 (current).
- Supports multiple authentication methods:
    - Password
    - Public key (most common)
    - Keyboard-interactive
    - Host-based
    - Challenge-response
    - GSSAPI
## Public Key Authentication (How it Works)

1. Server sends **public host key** → client verifies identity.
2. Client decrypts a challenge using its **private key**.
3. Server confirms it and grants access.
4. Passphrase unlocks private key once per session.
## Default OpenSSH Settings (Important)

```shell-session
Ssil3nce@htb[/htb]$ cat /etc/ssh/sshd_config  | grep -v "#" | sed -r '/^\s*$/d'

Include /etc/ssh/sshd_config.d/*.conf
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
PrintMotd no
AcceptEnv LANG LC_*
Subsystem       sftp    /usr/lib/openssh/sftp-server
```

## Dangerous Settings to Look For

- `PasswordAuthentication yes` → brute-force possible.
- `PermitEmptyPasswords yes` → empty passwords allowed.
- `PermitRootLogin yes` → direct root access.
- `Protocol 1` → insecure protocol.
- `X11Forwarding yes` → potential injection.
- `AllowTcpForwarding yes` → pivoting.
- `PermitTunnel yes` → tunneling.
- `DebianBanner yes` → unnecessary fingerprinting info.
## Footprinting SSH

### 1. Identify version

Use **ssh-audit** to fingerprint:

```shell-session
Ssil3nce@htb[/htb]$ git clone https://github.com/jtesta/ssh-audit.git && cd ssh-audit
Ssil3nce@htb[/htb]$ ./ssh-audit.py 10.129.14.132
```

You want:
- Protocol version
- OpenSSH version
- Weak algorithms
- Deprecated host keys

### 2. Check allowed authentication methods
#### Change Authentication Method

```shell-session
Ssil3nce@htb[/htb]$ ssh -v cry0l1t3@10.129.14.132
```

For potential brute-force attacks, we can specify the authentication method with the SSH client option `PreferredAuthentications`.
```shell-session
Ssil3nce@htb[/htb]$ ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```

### 3. Banner interpretation
- `SSH-1.99-OpenSSH_3.9p1` → supports SSH-1 and SSH-2.
- `SSH-2.0-OpenSSH_8.2p1` → SSH-2 only.