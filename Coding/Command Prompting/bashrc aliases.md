# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

#alias ccat='pygmentize -O style=monokai -f terminal -g'
#source ~/git-prompt.sh
#export PS1='\[\033[01;32m\]\u\[\033[01;34m\] \w\[\033[31m\]$(__git_ps1 " (%s)")\[\033[01;34m\]$\[\033[00m\] '
#alias cmd='/C/Windows/System32/cmd.exe'
#HISTSIZE=5000
#HISTFILESIZE=5000
#export HISTCONTROL=erasedups
#export HISTSIZE=10000
#shopt -s histappend
#source ~/.git-completion.bash
#source ~/.bash_prompt
#source ~/.bash_profile
#source ~/.screenrc
#source ~/.bash_history
#force_color_prompt=yes
# dir > unzipped.txt  save as text
#Get-Content C:\Scripts\Test.txt
alias ll='ls -l'
#bower
alias bwu='bower update'
alias bws='bower search'
alias bwi='bower install'
#alias -s='--save'
#gulp
alias gg='gulp'
#export HISTSIZE=999
#export HISTFILESIZE=999
#COLUMNS=999
#lines=999
#HISTSIZE=1000
#HISTFILESIZE=2000
#export HISTCONTROL=ignoreboth  # Ignore dups and commands that start with a space - they won't get added to history
#export HISTFILESIZE=50000 # Keep up to 5000 lines in history (default is 500)
#export HISTSIZE=50000     # Keep up to 5000 commands in shell history (default is 500)
#shopt -s histappend      # append to the history file, don't overwrite it
#shopt -s cmdhist         # In history file, combine multi-line comamnds into one line
#stty stop ""             # Disable the default meaning of C-s so it can be used for incremental search forward
#bind space:magic-space   # Space dynamically expands any ! history expansions
# set -o vi              # Uncomment this to get vi-style key bindings

#make new project
#alias lrn='laravel new'

#alias
alias al='alias'
#php artisan
alias pa='php artisan'
alias pih='php artisan ide-helper:generate'
alias pimt='php artisan ide-helper:meta'
alias pim='php artisan ide-helper:models'
# laravel new-app
# alias laravel="git clone -o laravel -b develop https://github.com/laravel/laravel.git"
alias pun="phpunit"

#Available commands:
alias pc='php artisan clear-compiled'
#Remove the compiled class file
alias pd='php artisan down'
#Put the application into maintenance mode
alias pe='php artisan env'
#Display the current framework environment
alias ph='php artisan help'
#Displays help for a command
alias pi='php artisan inspire'
#Display an inspiring quote
alias pl='php artisan list'
#Lists commands
alias pm='php artisan migrate'
#Run the database migrations
alias po='php artisan optimize'
#Optimize the framework for better performance
alias ps='php artisan serve'
#Serve the application on the PHP development server
alias pt='php artisan tinker'
#Interact with your application
alias pu='php artisan up'
#Bring the application out of maintenance mode
alias pb='php artisan about'
#Display an overview of configuration application

#app
alias pan='php artisan app:name'
#Set the application namespace

#auth
alias pau='php artisan auth:clear-resets'
#Flush expired password reset tokens

#cache
alias pcc='php artisan cache:clear'
#Flush the application cache
alias pct='php artisan cache:table'
#Create a migration for the cache database table

#config
alias pcoc='php artisan config:cache'
#Create a cache file for faster configuration loading
alias pcocl='php artisan config:clear'
#Remove the configuration cache file

#db
alias pdbs='php artisan db:seed'
#Seed the database with records

#debugbar
alias pdebc='php artisan debugbar:clear'
#Clear the Debugbar Storage

#event
alias peg='php artisan event:generate'
#Generate the missing events and listeners based on registration

#handler
alias phc='php artisan handler:command'
#Create a new command handler class
alias phe='php artisan handler:event'
#Create a new event handler class

#key
alias pkgg='php artisan key:generate'
#Set the application key

# make
alias kcm='php artisan make:command'
#Create a new command class
alias kfc='php artisan make:factory'
#Create a new Factory class
alias kcn='php artisan make:console'
#Create a new Artisan command
alias kcl='php artisan make:controller'
#Create a new resource controller class
alias ke='php artisan make:event'
#Create a new event class
alias kj='php artisan make:job'
#Create a new job class
alias kl='php artisan make:listener'
#Create a new event listener class
alias kmd='php artisan make:middleware'
#Create a new middleware class
alias kmi='php artisan make:migration'
#Create a new migration file
alias kmp='php artisan make:migration:pivot'
#Create a new migration pivot class
alias kms='php artisan make:migration:schema'
#Create a new migration class, and apply schema at the same time
alias kmo='php artisan make:model'
#Create a new Eloquent model class
alias kmm='php artisan make:mail'
#Create a new Mail model class
alias kmqt='php artisan make:queue-table'
#Create a new queue table class
alias kpr='php artisan make:provider'
#Create a new service provider class
alias kre='php artisan make:request'
#Create a new form request class
alias ksd='php artisan make:seed'
#Create a new database seed class
alias ksdr='php artisan make:seeder'
#Create a new seeder class
alias krs='php artisan make:resource'
#Create a new resource class
alias kcp='php artisan make:component'
#Create a new component for blade

#migrate
alias gin='php artisan migrate:install'
#Create the migration repository
alias grf='php artisan migrate:refresh'
#Rollbacks each of your migration batches then rerun all the migrations
alias grs='php artisan migrate:reset'
#Rollback all database migrations
alias grl='php artisan migrate:rollback'
#Rollback the last database migration
alias gst='php artisan migrate:status'
#Show the status of each migration
alias gfr='php artisan migrate:fresh'
#Reset and re-run all migrations
alias gfrs='php artisan migrate:fresh --seed'
#Reset and re-run all migrations and give seed to database

#queue
alias qfa='php artisan queue:failed'
#List all of the failed queue jobs
alias qfat='php artisan queue:failed-table'
#Create a migration for the failed queue jobs database table
alias qfl='php artisan queue:flush'
#Flush all of the failed queue jobs
alias qfo='php artisan queue:forget'
#Delete a failed queue job
alias qli='php artisan queue:listen'
#Listen to a given queue'
alias qrs='php artisan queue:restart'
#Restart queue worker daemons after their current job
alias qrt='php artisan queue:retry'
#Retry a failed queue job
alias qsu='php artisan queue:subscribe'
#Subscribe a URL to an Iron.io push queue
alias qta='php artisan queue:table'
#Create a migration for the queue jobs database table
alias qwo='php artisan queue:work'
#Process the next job on a queue

#route
alias rca='php artisan route:cache'
#Create a route cache file for faster route registration
alias rcl='php artisan route:clear'
#Remove the route cache file
alias rli='php artisan route:list'
#List all registered routes

#schedule
alias sru='php artisan schedule:run'
#Run the scheduled commands

#session
alias sta='php artisan session:table'
#Create a migration for the session database table

#vendor
alias vpu='php artisan vendor:publish'
#Publish any publishable assets from vendor packages

#view
alias vcl='php artisan view:clear'
#Clear all compiled view files

####composer
alias .c="composer"
alias .ch="composer help"
alias .cpd="composer create-project --prefer-dist"
alias .csu="sudo composer self-update"
alias .cu="composer update"
alias .ci="composer install"
alias .cr="composer require"
alias .csh="composer show"
alias .cse="composer search"
alias .cdu="composer dump-autoload"
alias .cduo="composer dump-autoload -o"

#tinker
alias pat="php artisan tinker"

#mysql
alias .m="mysql -u root -p"
alias .ms="show databases;"
alias .msb="subscribe"
alias .ms="selec * from"

# Git
alias ga="git add"
alias gaa="git add ."
alias gc="git commit -m"
alias gp="git push"
alias gs="git status"
alias gl="git log"
alias gpl="git pull"
#alias gs='git status '
#alias ga='git add '
#alias gb='git branch '
#alias gc='git commit'
#alias gd='git diff'
#alias go='git checkout '
#alias gk='gitk --all&'
#alias gx='gitx --all'
#alias got='git '
#alias get='git '
alias grv="git remote -v"
alias gbr="git branch -r"

# Directory
alias ..="cd ../"
alias ...="cd ../../"
alias ....="cd ../../../"
alias .....="cd ../../../../"

# Dump SQL
alias dump="cd ~/dumpsql"

# NPM Command
alias rdev="npm run dev"

# Pest
alias cpt="php artisan pest:test"

# Proyek path
alias proj="cd /mnt/windows-d/Proyek"

# ------------------------------------------------------------
#-- Path Laravel
#alias laravel="cd /mnt/windows-d/Proyek/Laravel"
alias pnm="cd /mnt/windows-d/Proyek/Laravel/graceperiode"
alias portaljob="cd /mnt/windows-d/Proyek/Laravel/portaljob"
alias ~="cd ~"
alias jeki="cd /mnt/windows-d/Proyek/Laravel/jeki"
alias kastara="cd /mnt/windows-d/Proyek/Laravel/kastara"
alias praditasari="cd /mnt/windows-d/Proyek/Laravel/praditasari"
alias filament="cd /mnt/windows-d/Proyek/Laravel/_packages/filament"
alias skripsi="cd /mnt/windows-d/Proyek/Laravel/okration"
alias ebhan="cd /mnt/windows-d/Proyek/Laravel/opname"
alias wpas="cd /mnt/windows-d/Proyek/Laravel/warehouse-pas"
alias pasd="cd /mnt/windows-d/Proyek/Laravel/newpas-master"
alias pas="cd /home/asrofil/Project/newpas-master"

#-- CodeIgniter
alias ci="cd /mnt/windows-d/Proyek/CodeIgniter"
alias labqc="cd /mnt/windows-d/Proyek/CodeIgniter/lab-qc"
alias cplt="cd /mnt/windows-d/Proyek/CodeIgniter/checksheet-plate"
alias logpc="cd /mnt/windows-d/Proyek/CodeIgniter/log-problem-customer"
alias logpi="cd /mnt/windows-d/Proyek/CodeIgniter/log-problem-internal"
alias investigation='cd /mnt/windows-d/Proyek/CodeIgniter/investigation-report'
alias testing="cd /mnt/windows-d/Proyek/CodeIgniter/testing-report"
alias bast="cd /mnt/windows-d/Proyek/CodeIgniter/bast"
alias pcv2="cd /mnt/windows-d/Proyek/CodeIgniter/production_control_v2"
alias wet="cd /mnt/windows-d/Proyek/CodeIgniter/wet-charging"

#-- GO
# alias go="cd /mnt/windows-d/Proyek/GO"

#-- Jupyter Lab
alias jupe="cd '/mnt/windows-d/Proyek/Jupyter Lab'"
# ------------------------------------------------------------

# ------------------------------------------------------------
# CodeIgniter
alias cn='composer create-project codeigniter4/appstarter'
alias testproject="cd ~/PhpstormProjects/test-project"
alias pss="php spark serve"

# ------------------------------------------------------------
alias catastrophy="bash ~/.tmux-catastrophy.sh"
alias armagedon="bash ~/.tmux-armagedon.sh"

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

# Auto-start tmux with custom layout
# if command -v tmux &> /dev/null && [ -z "$TMUX" ]; then
#     ~/.tmux-layouts.sh
#     exit
# fi

# Auto start tmux when open a terminal
if command -v tmux &> /dev/null && [ -z "$TMUX" ] && [ -z "$SSH_TTY" ]; then
    if tmux has-session -t main 2>/dev/null; then
        tmux attach-session -t main
    else
        SESSION="main"
        tmux new-session -d -s $SESSION -n "main" 'clear'

        # Split kiri-kanan: kiri 30% (≈80 char), kanan 70%
        tmux select-pane -t $SESSION:0
        tmux split-window -h -p 30 -t $SESSION

        # Jalankan btop di pane kiri
        tmux send-keys -t $SESSION:0.0 'btop' C-m

        # Pindah ke pane kanan
        tmux select-pane -t $SESSION:0.1

        # Split atas-bawah: atas 41%, bawah 59% (≈26 baris)
        tmux split-window -v -p 59 -t $SESSION:0.1

        # Jalankan neofetch di pane kanan bawah
        tmux send-keys -t $SESSION:0.2 'neofetch' C-m

        # Pindah ke pane kanan atas
        tmux select-pane -t $SESSION:0.1

        # Split pane kanan atas jadi kiri-kanan 50-50
        tmux split-window -h -p 50 -t $SESSION:0.1

        # Clear di kedua pane atas
        tmux send-keys -t $SESSION:0.1 'clear' C-m
        tmux send-keys -t $SESSION:0.3 'clear' C-m

        # Resize pane untuk presisi (opsional)
        tmux resize-pane -t $SESSION:0.0 -x 80   # Lebar 80
        tmux resize-pane -t $SESSION:0.2 -y 26   # Tinggi 26
        tmux resize-pane -t $SESSION:0.1 -y 18   # Tinggi 18

        # Fokus ke pane kerja (kiri atas)
        tmux select-pane -t $SESSION:0.1

        # Attach
        tmux attach-session -t $SESSION
    fi
fi

# Tmux session aquarium
alias aqua='tmux new-session -s aquarium -d && tmux send-keys -t aquarium "asciiquarium" C-m && tmux attach -t aquarium'

export PATH=$PATH:/home/asrofil/.spicetify
export PATH=$PATH:$HOME/spicetify

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
export PATH="$HOME/.local/bin:$PATH"
export PATH="$HOME/.local/bin:$PATH"
export PATH="$HOME/.local/bin:$PATH"
export PATH=$PATH:$HOME/.local/bin
export PATH="$HOME/.local/bin:$PATH"
export PATH="$HOME/.local/bin:$PATH"

# Composer global bin (WAJIB buat laravel)
export PATH="$HOME/.config/composer/vendor/bin:$PATH"

# ========================================
# Docker Aliases - Format Rapih
# ========================================

# Docker ps dengan format table rapih
alias dps='docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"'

# Docker ps -a dengan format table rapih
alias dpsa='docker ps -a --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"'

# Docker ps hanya nama dan status
alias dstat='docker ps --format "table {{.Names}}\t{{.Status}}"'

# Docker ps dengan ID pendek (12 karakter)
alias dpsid='docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Image}}\t{{.Status}}" | head -c 12'

# Docker ps sorted by name
alias dps-sorted='docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}" | (sed -u 1q; sort -k1)'

# Docker ps hanya running
alias dps-running='docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"'

# Docker ps dengan created time
alias dps-created='docker ps -a --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.CreatedAt}}"'

# Docker images
alias dmgs='docker images'

# py env
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"

# GO env path
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin

alias scrcpy='/home/asrofil/scrcpy-linux-x86_64-v3.3.4/scrcpy --no-clipboard-autosync'

alias mendeley='/home/asrofil/mendeley/mendeley-reference-manager-2.144.0-x86_64.AppImage --no-sandbox'