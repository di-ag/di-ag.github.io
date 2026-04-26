---
layout: post
title:  Linux Mint 22.2 "Xia" — Wayland to X11 Incident Report
date:   2026-04-17 23:50:14 +0930
categories: linux experience
---

*This report was produced by Claude AI*<br/>

**System:** Linux Mint 22.2 "Xia" / Cinnamon Desktop  
**Issue:** System persistently booted into Wayland session; unable to switch back to X11 (Xorg)



## Problem Description

On a Linux Mint 22.2 "Xia" system running the Cinnamon desktop environment, the user attempted to switch from the Wayland session back to the default X11 (Xorg) session. The LightDM login screen did not display a session selection icon, making the standard switching method unavailable.

---

## Troubleshooting Steps

### Step 1 — Login Screen Session Selector (Failed)

**Action:** Attempted to click the session selector icon on the LightDM login screen to choose "Cinnamon (Default)".  
**Result:** No session selector icon was visible on the login screen. Method not applicable.

---

### Step 2 — System-Level LightDM Configuration (Failed)

**Action:** Edited `/etc/lightdm/lightdm.conf` and set the `user-session` value to `cinnamon`.

```ini
[Seat:*]
user-session=cinnamon
```

**Result:** System still booted into Wayland after reboot. Change had no effect.

---

### Step 3 — AccountsService User Configuration ✅ (Resolved)

**Action:** Inspected and edited the user-level session record managed by AccountsService.

```bash
sudo nano /var/lib/AccountsService/users/$USER
```

**Finding:** The file contained an incorrect session name:

```ini
Session=ubuntu
```

**Fix:** Changed the value to the correct Cinnamon X11 session name:

```ini
Session=cinnamon
```

**Result:** After reboot, the system successfully launched the Cinnamon X11 desktop environment.

---

## Root Cause

The file `/var/lib/AccountsService/users/$USER` contained a stale and incorrect session entry (`Session=ubuntu`). LightDM reads this file with the **highest priority** when determining which session to launch for a user — overriding any system-level configuration set in `/etc/lightdm/lightdm.conf`. As a result, modifications to the system config file had no effect.

---

## Resolution

```bash
sudo nano /var/lib/AccountsService/users/$USER
```

Change:
```ini
Session=ubuntu
```

To:
```ini
Session=cinnamon
```

Save the file and reboot the system.

---

## Configuration File Priority Reference

| Configuration File | Priority | Description |
|---|---|---|
| `/var/lib/AccountsService/users/$USER` | **Highest** | Per-user session record; read by LightDM first |
| `~/.dmrc` | High | Alternative per-user session preference file |
| `/etc/lightdm/lightdm.conf` | Low | System-wide default configuration |

> **Key Takeaway:** When troubleshooting persistent Wayland session issues on Linux Mint, always check `/var/lib/AccountsService/users/$USER` first. This file holds the highest-priority session setting and is the most commonly overlooked location.

---

## Verification

After reboot, confirm the active display server by running:

```bash
echo $XDG_SESSION_TYPE
```

Expected output: `x11`

---

## Additional Notes

- Cinnamon's Wayland support in Linux Mint 22.2 remains at **alpha stage** and is not recommended for daily use.
- If a similar issue occurs in the future, inspecting `/var/lib/AccountsService/users/$USER` should be the first diagnostic step.
- Linux Mint 22.x is expected to support X11 until at least 2029 (end of support date), so switching to Wayland is not required at this time.
