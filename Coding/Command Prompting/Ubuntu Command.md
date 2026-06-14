ph## Docker - SQL Server

### Create SQL Server Container
```bash
sudo docker run -e 'ACCEPT_EULA=Y' \
  -e 'SA_PASSWORD=Asrofil123!' \
  -p 1433:1433 \
  --name sql2022 \
  -d mcr.microsoft.com/mssql/server:2022-latest
```

### Start SQL Server Container
```bash
sudo docker start sql2022
```

### Stop SQL Server Container
```bash
sudo docker stop sql2022
```

---

## MySQL Configuration

**Credentials:**

- **Username:** `asrofil`
- **Password:** `Asrofil123!`

**Connect to MySQL:**
```bash
mysql -u asrofil -p
```

---

## System Administration

### View All Partitions
```bash
lsblk
```

### Configure Boot Loader (GRUB)
```bash
sudo nano /etc/default/grub
```

After editing, update GRUB:

```bash
sudo update-grub
```

### Switch PHP Version

```bash
sudo update-alternatives --config php
```

### Install application .deb
```bash
sudo apt install <path>
```

---

## Network & Process Management

### View Running Applications and Ports

```bash
# View all listening ports
sudo ss -tulpn

# Filter specific application (e.g., Apache)
sudo ss -tulpn | grep apache2

# Alternative using netstat
sudo netstat -tulpn
```

### View Application Path
```bash
which <app_name>

# Examples:
which php
which python3
which node
```

### Monitor System Resources
```bash
# Modern, feature-rich monitor
btop

# Classic process monitor
htop

# Basic process list
top
```

### Kill Running Process
```bash
# Kill by PID
kill -9 <PID>

# Kill by process name
pkill <process_name>

# Example:
kill -9 1234
pkill apache2
```

### Get Application Class Name (X11)

```bash
xprop | grep WM_CLASS

# Usage: Run command, then click on the window
```

---

## Tmux - Terminal Multiplexer

### Session Management

| Command                       | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| `tmux ls`                     | List all sessions                              |
| `tmux new-session -s <name>`  | Create new session                             |
| `tmux attach -t <name>`       | Attach to session                              |
| `tmux kill-session -t <name>` | Kill session                                   |
| `aqua`                        | Create and run aquarium session (custom alias) |

### Key Bindings

**Prefix Key:** `Ctrl+a`

#### Pane Management

|Keybinding|Action|
|---|---|
|`Ctrl+a` then `v`|Split vertical (left/right)|
|`Ctrl+a` then `x`|Split horizontal (top/bottom)|
|`Ctrl+a` then `q`|Close current pane|
|`Ctrl+a` then `h/j/k/l`|Navigate panes (Vim-style)|
|`Ctrl+a` then `H/J/K/L`|Resize panes|

#### Session Management

|Keybinding|Action|
|---|---|
|`Ctrl+a` then `s`|Switch between sessions|
|`Ctrl+a` then `d`|Detach from session|
|`Ctrl+a` then `$`|Rename current session|

#### Window Management

|Keybinding|Action|
|---|---|
|`Ctrl+a` then `c`|Create new window|
|`Ctrl+a` then `n`|Next window|
|`Ctrl+a` then `p`|Previous window|
|`Ctrl+a` then `w`|List windows|
|`Ctrl+a` then `,`|Rename current window|

#### Other

|Keybinding|Action|
|---|---|
|`Ctrl+a` then `r`|Reload tmux config|
|`Ctrl+a` then `P`|Load preset layout|
|`Ctrl+a` then `?`|Show all keybindings|

---

## Custom Aliases

### Tmux Aquarium Session

```bash
alias aqua='tmux new-session -s aquarium -d && \
  tmux send-keys -t aquarium "asciiquarium" C-m && \
  tmux attach -t aquarium'
```

**Usage:**

```bash
aqua  # Creates and attaches to aquarium session
```

---

## Quick Reference

### Docker Commands

```bash
docker ps                    # List running containers
docker ps -a                 # List all containers
docker images                # List images
docker logs <container>      # View container logs
docker exec -it <container> bash  # Enter container shell
```

### File Permissions

```bash
chmod +x <file>             # Make executable
chmod 755 <file>            # rwxr-xr-x
chmod 644 <file>            # rw-r--r--
```

### Service Management (systemd)

```bash
sudo systemctl start <service>
sudo systemctl stop <service>
sudo systemctl restart <service>
sudo systemctl status <service>
sudo systemctl enable <service>   # Auto-start on boot
sudo systemctl disable <service>
```

### Download file from notion
```bash
curl -L -o "Nama_File.drawio" "URL_PANJANG_ITU"
atau
save as > nama_file.drawio
```
---
### Telegram desktop muncul di semua window
> Contoh: atur agar jendela dengan kelas tertentu tidak muncul di taskbar
```bash
gsettings set org.gnome.shell.app-switcher current-workspace-only true
```

---
## Notes

- Always backup configurations before editing
- Use `sudo` for system-level operations
- Check service status before making changes
- Document any custom configurations for future reference