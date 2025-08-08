Setting up a **`systemd` timer** to run a command at a **specific time** is a clean and modern alternative to cron jobs. Here's a complete guide to creating a timer unit and a service unit that runs your command.

---

## 🧭 Overview

You’ll need **two files**:

1. **A `.service` unit** – defines the command to run.
2. **A `.timer` unit** – schedules when the `.service` should run.

---

## 📝 Step-by-Step: Run a Command at a Specific Time

Let’s say you want to run `/home/user/myscript.sh` **every day at 7:00 PM**.

---

### ✅ 1. Create the Service File

```bash
sudo nano /etc/systemd/system/myscript.service
```

Paste:

```ini
[Unit]
Description=Run my custom script

[Service]
Type=oneshot
ExecStart=/home/user/myscript.sh
```

> 🔐 Make sure the script is executable:

```bash
chmod +x /home/user/myscript.sh
```

---

### ✅ 2. Create the Timer File

```bash
sudo nano /etc/systemd/system/myscript.timer
```

Paste:

```ini
[Unit]
Description=Run my custom script at 7 PM daily

[Timer]
OnCalendar=*-*-* 19:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

**Explanation:**

* `OnCalendar=*-*-* 19:00:00` → every day at 19:00 (7 PM)
* `Persistent=true` → runs missed executions if the system was off

---

### ✅ 3. Enable and Start the Timer

```bash
sudo systemctl daemon-reexec       # optional but safe
sudo systemctl daemon-reload
sudo systemctl enable myscript.timer
sudo systemctl start myscript.timer
```

---

### ✅ 4. Check Timer Status

```bash
systemctl list-timers
```

Or check just your timer:

```bash
systemctl status myscript.timer
```

---

## 🧪 Testing It Now

Want to test your service right away?

```bash
sudo systemctl start myscript.service
```

---

## 📂 Optional: Use `~/.config/systemd/user` (for user-level timers)

If you don't need root, you can create user-specific timers like this:

1. Put the `.service` and `.timer` files in:

   ```
   ~/.config/systemd/user/
   ```

2. Then reload and enable it:

```bash
systemctl --user daemon-reload
systemctl --user enable myscript.timer
systemctl --user start myscript.timer
```

3. Show user timers:

```bash
systemctl --user list-timers
```

> Note: You may need to enable lingering for timers to run when you're logged out:

```bash
sudo loginctl enable-linger $USER
```

---

## 🧠 Summary

| File Type  | Purpose                    |
| ---------- | -------------------------- |
| `.service` | Defines **what** to run    |
| `.timer`   | Defines **when** to run it |

---
