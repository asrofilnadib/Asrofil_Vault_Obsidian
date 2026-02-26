# Bash Aliases & Configuration Guide

## Table of Contents

- [[#Laravel Artisan Aliases|Laravel Artisan Aliases]]
- [[#Composer Aliases|Composer Aliases]]
- [[#Git Aliases|Git Aliases]]
- [[#Directory Navigation|Display Navigation]]
- [[#Project Path Shortcuts|Projects Path Shortcuts]]
- [[#NPM & Testing|NPM & Testing]]
- [[#Tmux Session Management|Tmux Session Management]]
- [[#General Commands|General Commands]]
- [[#Configuration Tips|Configuration Tips]]

---

## Laravel Artisan Aliases

### Basic Commands

|Alias|Command|Description|
|---|---|---|
|`pa`|`php artisan`|Base artisan command|
|`pl`|`php artisan list`|List all commands|
|`ph`|`php artisan help`|Display help|
|`pi`|`php artisan inspire`|Display inspiring quote|
|`pb`|`php artisan about`|Application overview|
|`pt`|`php artisan tinker`|Interactive shell|
|`pat`|`php artisan tinker`|Alternative tinker|

### Application Management

|Alias|Command|Description|
|---|---|---|
|`pd`|`php artisan down`|Enable maintenance mode|
|`pu`|`php artisan up`|Disable maintenance mode|
|`pe`|`php artisan env`|Show environment|
|`pc`|`php artisan clear-compiled`|Clear compiled files|
|`po`|`php artisan optimize`|Optimize application|
|`ps`|`php artisan serve`|Start dev server|

### Cache Management

|Alias|Command|Description|
|---|---|---|
|`pcc`|`php artisan cache:clear`|Clear application cache|
|`pct`|`php artisan cache:table`|Create cache table migration|
|`pcoc`|`php artisan config:cache`|Cache configuration|
|`pcocl`|`php artisan config:clear`|Clear config cache|
|`vcl`|`php artisan view:clear`|Clear compiled views|

### Database & Migrations

|Alias|Command|Description|
|---|---|---|
|`pm`|`php artisan migrate`|Run migrations|
|`gin`|`php artisan migrate:install`|Create migration table|
|`grf`|`php artisan migrate:refresh`|Rollback & re-migrate|
|`grs`|`php artisan migrate:reset`|Rollback all migrations|
|`grl`|`php artisan migrate:rollback`|Rollback last migration|
|`gst`|`php artisan migrate:status`|Show migration status|
|`gfr`|`php artisan migrate:fresh`|Drop all & re-migrate|
|`gfrs`|`php artisan migrate:fresh --seed`|Fresh migrate with seed|
|`pdbs`|`php artisan db:seed`|Seed database|

### Code Generation - Make Commands

|Alias|Command|Description|
|---|---|---|
|`kcm`|`php artisan make:command`|Create command|
|`kcn`|`php artisan make:console`|Create console command|
|`kcl`|`php artisan make:controller`|Create controller|
|`kmo`|`php artisan make:model`|Create model|
|`kmi`|`php artisan make:migration`|Create migration|
|`kmp`|`php artisan make:migration:pivot`|Create pivot migration|
|`kms`|`php artisan make:migration:schema`|Create migration with schema|
|`kfc`|`php artisan make:factory`|Create factory|
|`ksdr`|`php artisan make:seeder`|Create seeder|
|`ksd`|`php artisan make:seed`|Create seed|
|`kre`|`php artisan make:request`|Create request|
|`kmd`|`php artisan make:middleware`|Create middleware|
|`kpr`|`php artisan make:provider`|Create service provider|
|`ke`|`php artisan make:event`|Create event|
|`kl`|`php artisan make:listener`|Create listener|
|`kj`|`php artisan make:job`|Create job|
|`kmm`|`php artisan make:mail`|Create mail class|
|`krs`|`php artisan make:resource`|Create resource|
|`kcp`|`php artisan make:component`|Create component|
|`kmqt`|`php artisan make:queue-table`|Create queue table|

### Queue Management

|Alias|Command|Description|
|---|---|---|
|`qwo`|`php artisan queue:work`|Process queue jobs|
|`qli`|`php artisan queue:listen`|Listen to queue|
|`qrs`|`php artisan queue:restart`|Restart queue workers|
|`qfa`|`php artisan queue:failed`|List failed jobs|
|`qrt`|`php artisan queue:retry`|Retry failed job|
|`qfo`|`php artisan queue:forget`|Forget failed job|
|`qfl`|`php artisan queue:flush`|Flush failed jobs|
|`qfat`|`php artisan queue:failed-table`|Create failed jobs table|
|`qta`|`php artisan queue:table`|Create queue table|
|`qsu`|`php artisan queue:subscribe`|Subscribe to Iron.io push queue|

### Route Management

|Alias|Command|Description|
|---|---|---|
|`rli`|`php artisan route:list`|List all routes|
|`rca`|`php artisan route:cache`|Cache routes|
|`rcl`|`php artisan route:clear`|Clear route cache|

### Authentication & Session

|Alias|Command|Description|
|---|---|---|
|`pau`|`php artisan auth:clear-resets`|Clear password resets|
|`sta`|`php artisan session:table`|Create session table|

### Other Commands

|Alias|Command|Description|
|---|---|---|
|`pan`|`php artisan app:name`|Set app namespace|
|`pkgg`|`php artisan key:generate`|Generate app key|
|`peg`|`php artisan event:generate`|Generate events/listeners|
|`vpu`|`php artisan vendor:publish`|Publish vendor assets|
|`sru`|`php artisan schedule:run`|Run scheduled commands|
|`pdebc`|`php artisan debugbar:clear`|Clear debugbar storage|

### IDE Helper (Development)

|Alias|Command|Description|
|---|---|---|
|`pih`|`php artisan ide-helper:generate`|Generate helper|
|`pimt`|`php artisan ide-helper:meta`|Generate meta|
|`pim`|`php artisan ide-helper:models`|Generate model hints|

### Testing

|Alias|Command|Description|
|---|---|---|
|`pun`|`phpunit`|Run PHPUnit tests|
|`cpt`|`php artisan pest:test`|Run Pest tests|

### Project Creation

|Alias|Command|Description|
|---|---|---|
|`lrn`|`laravel new`|Create new Laravel project|
|`ln`|`laravel new`|Alternative (Windows)|

---

## Composer Aliases

|Alias|Command|Description|
|---|---|---|
|`.c`|`composer`|Base composer command|
|`.ch`|`composer help`|Show help|
|`.ci`|`composer install`|Install dependencies|
|`.cu`|`composer update`|Update dependencies|
|`.cr`|`composer require`|Require package|
|`.csh`|`composer show`|Show packages|
|`.cse`|`composer search`|Search packages|
|`.cpd`|`composer create-project --prefer-dist`|Create project|
|`.csu`|`sudo composer self-update`|Update composer|
|`.cdu`|`composer dump-autoload`|Regenerate autoload|
|`.cduo`|`composer dump-autoload -o`|Optimized autoload|

**Examples:**

```bash
.ci                           # Install dependencies
.cr laravel/sanctum          # Install Sanctum
.cu                           # Update all packages
.cdu                          # Regenerate autoload
```

---

## Git Aliases

|Alias|Command|Description|
|---|---|---|
|`ga`|`git add`|Stage file|
|`gaa`|`git add .`|Stage all files|
|`gc`|`git commit -m`|Commit with message|
|`gp`|`git push`|Push to remote|
|`gs`|`git status`|Show status|
|`gl`|`git log`|Show log|

**Examples:**

```bash
ga file.php               # Stage specific file
gaa                       # Stage all changes
gc "feat: add feature"    # Commit with message
gp origin main            # Push to main branch
gs                        # Check status
gl                        # View commit history
```

**See full Git commands in:** [Git Commands Reference](https://claude.ai/chat/1508e4ec-6e1d-4a7e-a227-6714cbd47417#)

---

## Directory Navigation

### Basic Navigation

|Alias|Command|Description|
|---|---|---|
|`..`|`cd ../`|Up one directory|
|`...`|`cd ../../`|Up two directories|
|`....`|`cd ../../../`|Up three directories|
|`.....`|`cd ../../../../`|Up four directories|
|`~`|`cd ~`|Go to home directory|

**Examples:**

```bash
..        # Go to parent directory
...       # Go up 2 levels
~         # Go to home
```

---

## Project Path Shortcuts

### Ubuntu/Linux Paths

#### Main Project Directory

```bash
alias proj="cd /mnt/windows-d/Proyek"
```

#### Laravel Projects

```bash
alias laravel="cd /mnt/windows-d/Proyek/Laravel"
alias pnm="cd /mnt/windows-d/Proyek/Laravel/graceperiode"
alias portaljob="cd /mnt/windows-d/Proyek/Laravel/portaljob"
alias jeki="cd /mnt/windows-d/Proyek/Laravel/jeki"
alias kastara="cd /mnt/windows-d/Proyek/Laravel/kastara"
alias praditasari="cd /mnt/windows-d/Proyek/Laravel/praditasari"
alias filament="cd /mnt/windows-d/Proyek/Laravel/_packages/filament"
alias skripsi="cd /mnt/windows-d/Proyek/Laravel/okration"
alias ebhan="cd /mnt/windows-d/Proyek/Laravel/opname"
alias wpas="cd /mnt/windows-d/Proyek/Laravel/warehouse-pas"
alias pasd="cd /mnt/windows-d/Proyek/Laravel/newpas-master"
alias pas="cd /home/asrofil/Project/newpas-master"
```

#### CodeIgniter Projects

```bash
alias ci="cd /mnt/windows-d/Proyek/CodeIgniter"
alias labqc="cd /mnt/windows-d/Proyek/CodeIgniter/lab-qc"
alias cplt="cd /mnt/windows-d/Proyek/CodeIgniter/checksheet-plate"
alias logpc="cd /mnt/windows-d/Proyek/CodeIgniter/log-problem-customer"
alias logpi="cd /mnt/windows-d/Proyek/CodeIgniter/log-problem-internal"
alias investigation="cd /mnt/windows-d/Proyek/CodeIgniter/investigation-report"
alias testing="cd /mnt/windows-d/Proyek/CodeIgniter/testing-report"
alias bast="cd /mnt/windows-d/Proyek/CodeIgniter/bast"
alias pcv2="cd /mnt/windows-d/Proyek/CodeIgniter/production_control_v2"
alias wet="cd /mnt/windows-d/Proyek/CodeIgniter/wet-charging"
```

#### Other Projects

```bash
alias go="cd /mnt/windows-d/Proyek/GO"
alias jupe="cd '/mnt/windows-d/Proyek/Jupyter Lab'"
alias testproject="cd ~/PhpstormProjects/test-project"
```

### Windows Paths

#### Main Project Directory

```bash
alias proj="cd D:/Proyek"
```

#### Laravel Projects

```bash
alias laravel="cd D:/Proyek/Laravel"
alias pnm="cd D:/Proyek/Laravel/graceperiode"
alias portaljob="cd D:/Proyek/Laravel/portaljob"
alias jeki="cd D:/Proyek/Laravel/jeki"
alias kastara="cd D:/Proyek/Laravel/kastara"
alias praditasari="cd D:/Proyek/Laravel/praditasari"
alias filament="cd D:/Proyek/Laravel/_packages/filament"
alias skripsi="cd D:/Proyek/Laravel/capaian_karya"
alias ebhan="cd D:/Proyek/Laravel/opname"
```

#### CodeIgniter Projects

```bash
alias ci="cd D:/Proyek/CodeIgniter"
alias labqc="cd D:/Proyek/CodeIgniter/lab-qc"
alias cplt="cd D:/Proyek/CodeIgniter/checksheet-plate"
alias logp="cd D:/Proyek/CodeIgniter/log-problem"
alias logpi="cd D:/Proyek/CodeIgniter/log-problem-internal"
```

#### Other Projects

```bash
alias jupe="cd 'D:/Proyek/Jupyter Lab'"
alias testproject="cd ~/PhpstormProjects/test-project"
```

**Usage:**

```bash
# Navigate to projects quickly
laravel      # Go to Laravel directory
pnm          # Go to graceperiode project
ci           # Go to CodeIgniter directory
labqc        # Go to lab-qc project
```

---

## NPM & Testing

### NPM Commands

```bash
alias rdev="npm run dev"     # Run development server
```

### CodeIgniter

```bash
alias cn="composer create-project codeigniter4/appstarter"  # Create new CI4 project
alias pss="php spark serve"                                  # Start CI4 server
```

### Legacy Tools (Optional)

```bash
# Bower (deprecated, but kept for legacy projects)
alias bwu="bower update"
alias bws="bower search"
alias bwi="bower install"

# Gulp
alias gg="gulp"
```

---

## Tmux Session Management

### Session Aliases

```bash
# Aquarium session - auto-create and run asciiquarium
alias aqua='tmux new-session -s aquarium -d && \
  tmux send-keys -t aquarium "asciiquarium" C-m && \
  tmux attach -t aquarium'
```

**Usage:**

```bash
aqua        # Create aquarium session with asciiquarium
```

**Manual Tmux Commands:**

```bash
# Create new session
tmux new-session -s <session_name>

# List sessions
tmux ls

# Attach to session
tmux attach -t <session_name>

# Kill session
tmux kill-session -t <session_name>

# Switch between sessions (inside tmux)
Ctrl+a then s
```

**See full Tmux guide in:** [Ubuntu Development Notes - Tmux Section](https://claude.ai/chat/1508e4ec-6e1d-4a7e-a227-6714cbd47417#)

---

## General Commands

### MySQL

```bash
alias .m="mysql -u root -p"
alias .ms="show databases;"
alias .msb="subscribe"
```

### List Aliases

```bash
alias al="alias"          # Show all aliases
alias ll="ls -l"         # List detailed
alias la="ls -A"         # List all
```

---

## Configuration Tips

### Where to Add Aliases

#### Linux (Ubuntu/Debian)

```bash
nano ~/.bashrc
# Add your aliases at the end
# Then reload:
source ~/.bashrc
```

#### Windows Git Bash

```bash
nano ~/.bashrc
# Add your aliases
# Restart Git Bash
```

### Reload Configuration

```bash
source ~/.bashrc          # Reload bashrc
exec bash                 # Restart bash
```

### View Current Aliases

```bash
alias                     # Show all aliases
alias pa                  # Show specific alias
type pa                   # Show alias definition
```

### Disable/Remove Alias

```bash
unalias pa               # Temporarily disable
# To permanently remove: delete from ~/.bashrc
```

---

## Advanced Configurations

### Auto-start Tmux on Terminal Open

**Ubuntu (~/.bashrc):**

```bash
if command -v tmux &> /dev/null && [ -z "$TMUX" ] && [ -z "$SSH_TTY" ]; then
    if tmux has-session -t main 2>/dev/null; then
        tmux attach-session -t main
    else
        SESSION="main"
        tmux new-session -d -s $SESSION
        tmux split-window -h -p 70 -c "$HOME"
        tmux split-window -v -p 35 -t 1 -c "$HOME"
        tmux split-window -h -p 50 -t 1 -c "$HOME"
        tmux send-keys -t 0 'btop' C-m
        tmux send-keys -t 1 'cd ~' C-m
        tmux send-keys -t 2 'neofetch' C-m
        tmux send-keys -t 3 'cd ~' C-m
        tmux select-pane -t 1
        tmux attach-session -t $SESSION
    fi
fi
```

### Export PATH Variables

**Ubuntu example:**

```bash
export PATH=$PATH:/home/asrofil/.spicetify
export PATH=$PATH:$HOME/spicetify
export PATH="$HOME/.local/bin:$PATH"

# NVM (Node Version Manager)
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

### History Configuration

```bash
HISTCONTROL=ignoreboth    # Ignore duplicates & space-prefixed
HISTSIZE=1000            # Commands in memory
HISTFILESIZE=2000        # Commands in file
shopt -s histappend      # Append to history, don't overwrite
```

---

## Platform Differences

### Path Format

**Linux/Ubuntu:**

```bash
/mnt/windows-d/Proyek/Laravel
/home/asrofil/Project
```

**Windows:**

```bash
D:/Proyek/Laravel
C:/Users/asrofil/Project
```

### Command Differences

|Task|Ubuntu|Windows Git Bash|
|---|---|---|
|Edit bashrc|`nano ~/.bashrc`|`nano ~/.bashrc`|
|Reload|`source ~/.bashrc`|Restart terminal|
|Home directory|`/home/username`|`/c/Users/username`|
|Drive access|`/mnt/c/` `/mnt/d/`|`/c/` `/d/`|

---

## Quick Reference Card

### Most Used Aliases

```bash
# Laravel
pa          # php artisan
pm          # migrate
gfr         # fresh migrate
pcc         # clear cache
ps          # serve

# Composer
.ci         # install
.cu         # update
.cdu        # dump-autoload

# Git
gaa         # add all
gc "msg"    # commit
gp          # push

# Navigation
..          # up one level
laravel     # to Laravel dir
proj        # to project dir

# Tmux
aqua        # aquarium session
```

### Create Custom Alias

```bash
# Temporary (current session only)
alias myalias='command here'

# Permanent
echo "alias myalias='command here'" >> ~/.bashrc
source ~/.bashrc
```

### Example Custom Aliases

```bash
# Quick project setup
alias setup='composer install && npm install && php artisan key:generate'

# Clear all caches
alias clearall='php artisan optimize:clear && composer dump-autoload'

# Git shortcuts
alias gitundo='git reset --soft HEAD~1'
alias gitlog='git log --oneline --graph --all'

# Docker
alias dcu='docker compose up -d'
alias dcd='docker compose down'
alias dcl='docker compose logs -f'
```

---

## Best Practices

✅ **Do:**

- Group related aliases together
- Add comments for complex aliases
- Use descriptive names
- Keep aliases short but memorable
- Backup your .bashrc before major changes
- Document custom aliases

❌ **Don't:**

- Override system commands without good reason
- Use conflicting alias names
- Make aliases too complex (use functions instead)
- Forget to reload after changes

---

## Troubleshooting

### Alias Not Working

```bash
# Check if alias exists
type alias_name

# Check for typos in .bashrc
nano ~/.bashrc

# Reload configuration
source ~/.bashrc

# Check for conflicts
alias | grep alias_name
```

### Command Not Found

```bash
# Ensure path is in .bashrc
echo $PATH

# Check if command exists
which command_name

# For PHP/Composer issues
php -v
composer -v
```

### Path Issues (Windows in Linux)

```bash
# List mounted drives
ls /mnt/

# Check if drive is mounted
mount | grep windows
```

---

## Additional Resources

- [Laravel Debugging Guide]() - Full Laravel troubleshooting
- [Docker Commands Reference](https://claude.ai/chat/1508e4ec-6e1d-4a7e-a227-6714cbd47417#) - Complete Docker guide
- [Git Commands Reference](https://claude.ai/chat/1508e4ec-6e1d-4a7e-a227-6714cbd47417#) - Comprehensive Git guide
- [Ubuntu Development Notes](https://claude.ai/chat/1508e4ec-6e1d-4a7e-a227-6714cbd47417#) - System administration tips