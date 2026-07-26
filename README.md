# UFW Firewall Setup on Kali Linux

A quick reference for configuring `ufw` (Uncomplicated Firewall) on a Kali Linux box — useful for locking down a pentest VM or any host that needs a sane default-deny posture.

## 1. Install UFW

UFW isn't preinstalled on Kali, so grab it first.

```bash
sudo apt update
sudo apt install ufw -y
```

## 2. Set Default Policies

Deny all incoming traffic by default, allow all outgoing traffic.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

<img width="472" height="205" alt="Screenshot 2026-07-26 220820" src="https://github.com/user-attachments/assets/8612b9c1-471c-463c-ac5f-23b0dc687581" />


## 3. Allow Specific Services

Open up the ports you actually need.

```bash
sudo ufw allow ssh          # port 22
sudo ufw allow 80/tcp       # HTTP
sudo ufw allow 443/tcp      # HTTPS
```

<img width="358" height="303" alt="Screenshot 2026-07-26 220833" src="https://github.com/user-attachments/assets/287525e3-0f41-422c-8a2e-11a131ce489a" />

## 4. Restrict Access to a Specific Subnet

If you only want certain hosts (e.g. your local network) reaching the box, allow by subnet:

```bash
sudo ufw allow from 192.168.1.0/24
```

<img width="425" height="77" alt="Screenshot 2026-07-26 220841" src="https://github.com/user-attachments/assets/8f210f7f-9979-4c92-bfdf-440ed4ca6dc7" />


## 5. Enable the Firewall

```bash
sudo ufw enable
```

## 6. Check Status

```bash
sudo ufw status verbose
```

<img width="616" height="355" alt="Screenshot 2026-07-26 220849" src="https://github.com/user-attachments/assets/820f4a1c-6ccf-4111-97b3-b00d1b4b6b37" />


## 7. Deleting a Rule

Rules can be removed the same way they were added, prefixed with `delete`:

```bash
sudo ufw delete allow 80/tcp
```

```
Rule deleted
Rule deleted (v6)
```

After deletion, `sudo ufw status verbose` reflects the updated rule set:

<img width="636" height="411" alt="Screenshot 2026-07-26 220857" src="https://github.com/user-attachments/assets/5b0a7b28-de4d-4709-86b3-65061d7a8c1d" />


## Full Command Reference

```bash
# Install (usually not preinstalled on Kali)
sudo apt update
sudo apt install ufw -y

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow specific services
sudo ufw allow ssh              # port 22
sudo ufw allow 80/tcp            # HTTP
sudo ufw allow 443/tcp           # HTTPS

# Allow only from a specific IP/subnet (useful on a pentest VM)
sudo ufw allow from 192.168.1.0/24

# Enable the firewall
sudo ufw enable

# Check rules
sudo ufw status verbose

# Delete a rule
sudo ufw delete allow 80/tcp
```

---

**Notes:**
- UFW rules are applied to both IPv4 and IPv6 by default when using generic rules like `allow ssh`.
- Always double-check `ufw status verbose` after making changes to confirm the intended rules are in place.
- On a machine you're accessing remotely, allow SSH **before** enabling the firewall (or before setting `deny incoming`) to avoid locking yourself out.
