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

```
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
```

## 3. Allow Specific Services

Open up the ports you actually need.

```bash
sudo ufw allow ssh          # port 22
sudo ufw allow 80/tcp       # HTTP
sudo ufw allow 443/tcp      # HTTPS
```

Each command adds both an IPv4 and IPv6 rule:

```
Rule added
Rule added (v6)
```

## 4. Restrict Access to a Specific Subnet

If you only want certain hosts (e.g. your local network) reaching the box, allow by subnet:

```bash
sudo ufw allow from 192.168.1.0/24
```

```
Rule added
```

## 5. Enable the Firewall

```bash
sudo ufw enable
```

```
Firewall is active and enabled on system startup
```

## 6. Check Status

Basic status:

```bash
sudo ufw status
```

```
Status: active
```

Verbose status (shows defaults and every rule):

```bash
sudo ufw status verbose
```

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
Anywhere                   ALLOW IN    192.168.1.0/24
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
443/tcp (v6)                ALLOW IN    Anywhere (v6)
```

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

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
Anywhere                   ALLOW IN    192.168.1.0/24
22/tcp (v6)                ALLOW IN    Anywhere (v6)
443/tcp (v6)                ALLOW IN    Anywhere (v6)
```

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
