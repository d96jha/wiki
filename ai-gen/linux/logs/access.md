# 🔐 Viewing Access Attempt Logs in Linux

Yes, Linux **does keep logs of access attempts**—both successful and failed—through various system log files. These logs are useful for auditing, security monitoring, and understanding who accessed your system and when.

---

## 🕵️‍♂️ Where to Find Access Attempt Logs

Here are the key places and commands to check:

---

### 📂 1. `/var/log/auth.log` *(Debian/Ubuntu)*

This is the main log for authentication events like SSH logins, `sudo` usage, and PAM-related activity.

```bash
sudo less /var/log/auth.log
````

**Search for keywords:**

* `Accepted password` – successful login
* `Failed password` – failed login attempt
* `Invalid user` – login with a non-existent account
* `sudo` – usage of sudo

Example:

```bash
grep "Failed password" /var/log/auth.log
```

---

### 📂 2. `/var/log/secure` *(Red Hat / CentOS / Fedora)*

Same purpose as `auth.log` but on RHEL-based distributions.

```bash
sudo less /var/log/secure
```

---

### 📂 3. `/var/log/wtmp` and `/var/log/btmp` — Login Records

These are binary logs that record login history and failed login attempts.

#### ✅ Show successful logins:

```bash
last
```

#### ❌ Show failed login attempts:

```bash
sudo lastb
```

These commands display:

* Username
* IP address or hostname
* Timestamp

---

### 📂 4. `journalctl` (Systemd-based systems)

If your system uses **systemd**, you can use `journalctl` to access authentication logs.

#### Show SSH authentication messages:

```bash
sudo journalctl -u ssh
```

#### Filter by keyword:

```bash
sudo journalctl | grep "Failed password"
```

---

## 📍 Summary

| File / Command      | Description                 |
| ------------------- | --------------------------- |
| `/var/log/auth.log` | Auth logs (Debian/Ubuntu)   |
| `/var/log/secure`   | Auth logs (RHEL/Fedora)     |
| `last`              | Shows login history         |
| `lastb`             | Shows failed login attempts |
| `journalctl`        | Systemd journal access      |
| `grep`              | Filter logs by keyword      |

---

## 🚨 Real-Time Monitoring

Monitor login attempts live:

#### Tail auth log:

```bash
sudo tail -f /var/log/auth.log
```

#### Follow SSH logs (systemd):

```bash
sudo journalctl -f -u ssh
```

---

## 🔒 Optional: Protect Yourself

Want to receive alerts for login attempts or block attackers?
Look into tools like:

* `fail2ban`
* `sshguard`
* `auditd`

---
