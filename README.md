Hardened SSH 2026

Hardened SSH made with AI (OpenAI/ChatGPT). Human-controlled.
AI was used to generate a modern, secure, and annotated configuration.
All settings have been reviewed and tested manually to ensure compatibility and security.

📌 Features

Key-only login (no passwords)

Strong post-quantum safe cryptography (sntrup761x25519, curve25519)

Forwarding (TCP, Agent, StreamLocal) fully disabled

Brute force rate limiting (MaxAuthTries, MaxStartups)

Minimal attack surface (X11, TUN, compression disabled)

Modern OpenSSH defaults (2026)

Internal SFTP support

⚙️ Installation

Backup current SSH config:

sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

Copy the hardened configuration:

sudo cp hardend.ssh /etc/ssh/sshd_config

Generate strong host keys:

sudo rm /etc/ssh/ssh_host_*
sudo ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""

Remove weak Diffie-Hellman moduli (Debian/Ubuntu):

sudo awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.safe
sudo mv /etc/ssh/moduli.safe /etc/ssh/moduli

Restart SSH server:

sudo systemctl restart ssh

Test login with a separate session before closing your current session to avoid lockout.

🧪 Testing & Verification

Check SSH version:

ssh -V

Test connection:

ssh -i ~/.ssh/id_ed25519 youruser@yourserver

Run ssh-audit (optional, for auditing):

ssh-audit yourserver

Should report strong ciphers, secure key exchange, and no weak algorithms.

🔒 Extra Hardening Tips

Use Fail2Ban or ufw limit 22/tcp to throttle connection attempts.

Consider port knocking or moving SSH to a non-standard port.

Use YubiKey / hardware keys for key authentication.

Optional: SSH over WireGuard / VPN for additional layer of security.

Regularly audit your configuration and host keys.

📂 Repository Structure
Hardenedssh2026/
 ├─ hardend.ssh       # The hardened SSH configuration
 ├─ README.md         # This file
 ├─ install.sh        # Optional install script
 └─ hardening.md      # Optional documentation on hardening choices
⚠️ Disclaimer

Test all changes in a safe environment first.

Locking yourself out of SSH is possible if you misconfigure.

This configuration is human-reviewed, AI-generated suggestions are verified manually.
