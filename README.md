# Tracking packages I have installed on my distros:

![screenshot1](/scrot_1.png)
![screenshot2](/scrot_2.png)
![screenshot3](/scrot_3.png)

## Prompt String
+ Updated with fenced code block! Having it in plain text will have unintended escape sequences being interpreted by markdown since the prompt string utilizes backslash `\`.
```bash
PS1="\[\033[s\033[0;0H\033[0;45m\033[K\033[1;37m\h => \W\033[0m\033[u\]\[\033[0;43m\]\[\033[0;31m\]\[\033[1B\]\u ->\$ \[\033[1;33m\]"
export PS1
```

## Regolith
+ Changes directly applied to regolith DE configs

`~/.config/regolith/Xresources`
```bash
# Xresources file to config i3 within regolith

# fonts
i3-wm.font: pango:Tamzen 16

# i3 bar
#i3-wm.bar.font: pango:Tamzen 16

# i3 appearance
i3-wm.client.focused.color.child_border: "#d22c26"

# Wallpaper settings
gnome.wallpaper: /home/sheung/Pictures/spaceman.png

# Startup programs
i3-wm.program.1: /home/sheung/.config/polybar/launch.sh
```

`/etc/regolith/i3/config`
+ commented out the entire i3-bar section to disable it.
	+ Currently trying out Polybar
+ Changed terminal keybind (Super+Enter) to launch urxvt instead.
+ Change i3-wm.program.1 (polybar) to `exec_always`

```bash
## Launch // Terminal // <Super> Enter ##
set_from_resource $i3-wm.binding.terminal i3-wm.binding.terminal Return
bindsym $mod+$i3-wm.binding.terminal exec /usr/bin/urxvt

## User programs from Xresources
set_from_resource $i3-wm.program.1 i3-wm.program.1 :    
exec_always --no-startup-id $i3-wm.program.1
```

## Polybar
Installed by compiling from source.
+ Had to troubleshoot several issues.
	+ default centre widget `mpd` needed to be installed, which is available from `apt`
	+ some unicode characters require glyphs from `siji` font
		+ siji font could be grabbed from `https://github.com/stark/siji`
		+ however, after running `./install.sh` the font shows up on `xfontsel` but not on `fc-list siji`, and polybar would still emit `Dropping unmatched character...`
		+ Solved by removing file `sudo rm /etc/fonts/conf.d/70-no-bitmaps.conf`
		+ `fc-cache` then `fc-list siji` and siji is listed.
+ Create a launch.sh script for launch on i3 startup

`/home/sheung/.config/polybar/launch.sh`
```bash
#!/bin/bash    
    
echo "---" | tee -a /tmp/polybar1.log    
    
polybar example 2>&1 | tee -a /tmp/polybar1.log & disown    
    
echo "bars launched..."
```

## neofetch
+ I have tried setting custom images with `--w3m` which worked for the most part with `--size`, but results vary from terminal to terminal, so it's a bit tedious.
+ Would probably use macchina or fet.sh instead most of the time for simplicity.

## nnn file manager

`~/.bashrc`
```bash
# NNN settings
alias nnn="nnn -a -d"
NNN_PLUG="p:preview-tui;i:imgview;"
alias nnn-update-plugins="curl -Ls https://raw.githubusercontent.com/jarun/nnn/master/plugins/getplugs | bash"
export EDITOR="nvim"
```

## neovim

`~/.config/nvim/init.vim`
```bash
set number
set hlsearch
set tabstop=4
set shiftwidth=4
set noexpandtab
set autoindent
syntax on
nnoremap o o<Esc>
nnoremap O O<Esc>
```

## i3-gaps
+ Ended up not required since the display is shared with the Windows Host
+ Might tweak i3 on my Ubuntu server but the only GUI application I use is the browser anyway, so not much motivation for that.
	+ Might make a custom homepage for Firefox at some point, if I feel a sudden burst of joy and vigor.

## xorg
+ Did not work out of the box
+ Install VcXsrv on Windows Host
	+ Disable access control
+ Allow access to port 6000
+ Also tried allowing `wsl.exe` (Microsoft Windows Subsystem for Linux Launcher), `xlaunch.exe` through Windows Firewall.
+ `export DISPLAY=IPv4_of_Host:0.0`
+ Can test with `xeyes`
+ Launch with `xlaunch` on Host.

`~/.bashrc`
```bash
# Connect to Host Display (replace *Host_IPv4*)
export DISPLAY="*Host_IPv4*:0.0"
```

> Remember to change `$DISPLAY` when internet connection changes.
> There's also currently a problem with WSL2 not loading without an internet connection.

## kbd
+ used for chvt

## urxvt (rxvt-unicode)
+ Create `~/.Xresources` config file to config urxvt (same as xterm)
+ Use `xrdb ~/.Xresources` after configuring to reload and apply changes.
+ Using Mutiny colorscheme by pyratebeard (https://gitlab.com/pyratebeard/dotfiles/-/tree/master)
+ Struggled with getting fonts to work.
	+ Installed tamzen on Windows Host.
	+ Had Tamzen-font in VcXsrv/fonts.
	+ Copied Tamzen-font/ttp file contents into VcXsrv/fonts/TTP.
+ Unable to find or select tamzen font through `xset` or `xfontsel`.
+ Think what worked is the `xft:tamzen` line in `~/.Xresources`.

`~/.bashrc`
```bash
# Need to reload settings on start up for some reason
xrdb ~/.Xresources
```

`~/.Xresources`
```bash
! mutiny by pyratebeard (https://git.pyratebeard.net/?p=mutiny)
! |__ inspired by sourcerer by xero (https://sourcerer.xero.nu)

! special
*.foreground:   #bbbbbb
*.background:   #171717
*.cursorColor:  #bbbbbb

! black
*.color0:               #131313
*.color8:               #3d3a3b

! red
*.color1:               #883c43
*.color9:               #bb6767

! green
*.color2:               #899e3b
*.color10:              #d2d074

! yellow
*.color3:               #deb14f
*.color11:              #807b64

! blue
*.color4:               #6e6461
*.color12:              #909798

! magenta
*.color5:               #753747
*.color13:              #956c74

! cyan
*.color6:               #55776e
*.color14:              #71bb94

! white
*.color7:               #bbbbbb
*.color15:              #baba9e

! fonts
urxvt*font: xft:tamzen:pixelsize=18:antialias=false

urxvt*letterSpace: 0
urxvt*lineSpace: -1

! interface
urxvt*scrollBar: false
urxvt*cursorUnderline: false
urxvt*cursorBlink: true

! style
urxvt*fading: 10
urxvt*fadeColor: #1e1b1c

! clipboard
! changed from using urxvt-perls extensions to using xclip and native cp and paste
! link to urxvt-perls https://github.com/muennich/urxvt-perls
URxvt.perl-ext-common: default, clipboard, keyboard-select
URxvt.copyCommand: xclip -in -sel clip    
URxvt.pasteCommand: xclip -out -sel clip    
URxvt.keysym.Shift-Control-V: eval:paste_clipboard    
URxvt.keysym.Shift-Control-C: eval:selection_to_clipboard    
URxvt.iso14755: false    
URxvt.iso14755_52: false
```

## tmux
+ `tmux list-sessions` to find existing sessions
+ `tmux attach` to get to the sessions and kill them
+ could also use `killall tmux` or `tmux kill-server`
+ install tmux plugins manager (tpm) with `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

`~/.bashrc`
```bash
alias tpm-install="~/.tmux/plugins/tpm/bin/install_plugins"      
alias tpm-update="~/.tmux/plugins/tpm/bin/update_plugins all"    
alias tpm-remove="~/.tmux/plugins/tpm/bin/clean_plugins"
```

`~/.tmux.conf`
```bash
# Shameless ripoff of Pyratebeard's Mutiny configs (https://gitlab.com/pyratebeard/dotfiles/-/tree/master)

# pane switching    
unbind h    
unbind j    
unbind k    
unbind l    
bind h select-pane -L    
bind j select-pane -D    
bind k select-pane -U    
bind l select-pane -R    
    
# pane border    
set -g pane-border-style fg=black    
set -g pane-active-border-style fg=colour6    
    
# status bar    
set -g status-justify right    
set -g status-style bg=terminal    
set -g status-fg colour7    
set -g status-interval 5    
set -g status-right-length 100    
setw -g window-status-separator " "    

setw -g window-status-format "#[bg=colour241,fg=colour0] #I #[bg=colour241,fg=colour0]#W #[bg=colour0,fg=colour241]    ??????"    
setw -g window-status-current-format "#[bg=colour14,fg=colour0] #I #[bg=colour14,fg=colour0]#W #[bg=colour0,fg=colo    ur14]??????"    
set -g status-justify left    
set-option -g status-right '#[bg=colour0,fg=colour237]??????#[bg=colour236,fg=colour15]#{bond_device}#{online_status}#[    bg=colour237,fg=colour243] %Y%m#[bg=colour237,fg=colour3]%d#[fg=default]-#[fg=colour10]%u #[fg=colour7]%H%M #[bg=wh    ite,fg=colour237]???#[default]'
set-option -g status-left ''

# online and offline icon for tmux-online-status
set -g @online_icon "#[bg=colour237,fg=colour2]?????????#[bg=colour2,fg=black]online#[bg=colour237,fg=colour2]?????????#[default]"
set -g @offline_icon "#[bg=colour237,fg=colour1]?????????#[bg=colour1,fg=white]offline#[bg=colour237,fg=colour1]?????????#[default]"                           

# device names for tmux-bond-device
set -g @ethernet "#[bg=colour237,fg=colour7] hardwire #[default]"
set -g @wifi "#[bg=colour237,fg=colour7] airborne #[default]"

# tmux clock                      
set -g clock-mode-colour colour6

# plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-online-status'
set -g @plugin 'https://gitlab.com/pyratebeard/tmux-bond-device.git'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

## mutt

## jakym
+ had been using this for a while now
+ `sudo apt-get install -y python3-dev libasound2-dev`
+ `pip install simpleaudio`
+ had to manually install pip for some reason, thought it was bundled with python3.4 and greater.
	+ `sudo apt install pip`
+ `pip install jakym`
+ Need to put in some work for WSL2 audio to work, probably with the pulse audio workaround.

## rustup -> cargo
+ (!!) Install with rustup instead, since rustup bundles rustc and cargo.
+ else if you try to install rustup, it'll warn you potential $PATH conflict/confusion.

`~/.bashrc`
```bash
export PATH="$PATH:/home/$USER/.cargo/bin"
```

## macchina
+ powered by rust and cargo
+ `cargo install macchina`
+ because neofetch can't seem to load images properly (at least not some custom ascii)

`~/.bashrc`
```bash
alias macchina="macchina -t Helium -p -C Red --box-title \"[ Sean's Dirty Secrets ]\""
```

## fet.sh
+ for bloat haters
+ hosted at `https://github.com/6gk/fet.sh`
+ simple to set up, simple to use

## unzip
+ doesn't come preinstalled
+ needed for nnn preview-tui to work

## imv
+ needed for nnn imgview to work
+ need to go to plugin source to replace `imvr` to `imv`

## ruby
+ needed for jekyll
+ `sudo apt install ruby-full` (deb)
+ official documentation of jekyll points to Brightbox Ruby NG for Ubuntu
	+ hosted on `ppa:brightbox/ruby-ng`
	+ on Ubuntu the repo could be added through `sudo apt-add-repository`
+ on Kali however
	+ the official Kali repo shipped with install is stored at `/etc/apt/sources.list`
		+ format is `<Archive> <Mirror> <Branch> <Component>`
		+ currently on kali-rolling (default)
	+ additional repositories should be stored under `/etc/apt/sources.list.d/`
	+ naming convention: `repo-name.list`
+ `sudo gem update`

## jekyll
+ `sudo gem install jekyll bundler`
+ following the documentation on github pages, within the `gemfile` github-pages' version is set to `217`
	+ This will cause several version conflicts after running `bundle update` or `bundle install`.
	+ Deleting `gemfile.lock` and running `bundle install` then `bundle update` resolves the issue.
	+ run `bundle update github-pages` with bundle or `gem update github-pages` to update github-pages gem
+ test site locally with `bundle exec jekyll serve` (optional --livereload)
+ jerkyll posts must be name in `YEAR-MONTH-DAY-title.MARKUP`
+ could init new dir by:
	+ `jekyll new .` or
	+ `bundle init` -> `bundle add jekyll`
+ gem based themes have some directories (_assets, _includes, _layouts, _sass) stored inside the theme's gem file.
	+ use `bundle info --path *theme-name*` to find where those files are.
	+ writing your own layout files under the same name can override theme layouts.

By default `JEKYLL_ENV` is set to development. You can set the environment when running a command:
```bash
JEKYLL_ENV=production bundle exec jekyll serve
```

To only output the analytics script on development:
```jekyll
# JEKYLL_ENV is available on jekyll.environment
{% if jekyll.environment == "development" %}
	<sciprt src="my_analytic_script.js"></script>
{% endif %}
```

## lsof
+ got this because I forgot that I have a window running on tmux that was occupying a port
+ run `lsof -wni tcp:PORT` to get PID
+ run `kill -9 PID`

## tuir
+ install with pip: `pip install tuir`
+ install from source:
	+ `git clone https://gitlab.com/ajak/tuir.git`
	+ `cd tuir && python setup.py install`
+ associating MIME types with `tuir --copy-mailcap`
	+ Setup mailcap file, then launch tuir with `tuir --enable-media` flagm or set it in `tuir.cfg`
+ comment out line for vlc in `~/.config/tuir/mailcap` if using mpv

## feh
+ image viewer for tuir
+ get dependencies `apt-get install libcurl4-openssl-dev libx11-dev libxt-dev libimlib2-dev libxinerama-dev libjpeg-progs`
+ clone repo `git clone https://git.finalrewind.org/feh`
+ `cd feh && make` -> `sudo make install`

## mpv
+ video and media player for tuir
+ `sudo apt install mpv`

## surf
+ fall back browser
+ trouble getting it to work on WSL2.

## funsies

+ `curl -L rum.sh/ricebowl -o ricebowl`
	+ for testing and fun

+ cmatrix

+ figlet

+ nms

+ devilspie
	+ I do know that devilspie 2 exists, but I have had devilspie since last year, and it works perfectly fine, so I'm keeping it.
	+ create a file for each application that you want to apply transparency, i.e. `~/.devilspie/name-of-prog_transparent.ds`
	+ You can find the window_class of the application by opening a terminal next to the app you want to check, and run `xwininfo`
		+ It'll prompt you to click on the window you would like to check
		+ Get the window ID and run `xprop -id *window_id*`
		+ Find the WINDOW_CLASS section
		+ Note: There might be more than one entry. For example for me `urxvt` wouldn't work but `URxvt` works.
	+ Run `devilspie` to apply effect, or you could add a line in your `.bashrc` to run it on start up.

for example:
`firefox_transparent.ds`
```Lua
( if
		( contains ( window_class ) "Firefox" )
	( begin 
		spawn_async ( str "xprop -id "
			( window_xid ) " -f _NET_WM_WINDOW_OPACITY 32c -set _NET_WM_WINDOW_OPACITY 0xdfffffff"
			)
	)
)
```

## Misc

Desktop failed to boot after I tried to create config files for i3 in `~`.
+ Naturally my approach was to remove those config files and restart.
	+ This led me to try and remove the files through tty.
+ However, while trying to login to tty, I realized that my keyboard layout has been changed to Dvorak for some reason.
	+ I had used Dvorak before, but have switched back to Qwerty after a while.
	+ The Dvorak layout has since been removed, which is why it was strange to have the layout suddenly set to Dvorak.
+ At this point I had two options, to remove the files while having a Dvorak keymap open on another screen, or try to change the keyboard back to normal and remove the files.
+ I ended up using ssh to remove the files, and change the keyboard through `/etc/default/keyboard`
	+ Within the file, `XKBLAYOUT` shows the current keyboard, and `XKBVARIANT` was showing "Dvorak", so I simply emptied the `XKBVARIANT` to an empty string.
+ Rebooted after and everything is back to normal.

+ Since polybar battery status is inaccurate and does not refresh, using acpi temporarily to check battery status.
