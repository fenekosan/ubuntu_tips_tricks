# Ubuntu 23.10 tips & tricks 

## 1. Disable snap

Get installed apps
```sh
snap list
```

Remove snap packages in the following order.
```sh
sudo snap remove --purge firefox
sudo snap remove --purge snap-store
...
```
Finally, remove the snap daemon via apt command.
```sh
sudo apt remove --autoremove snapd
```

Disable snap for apt

```sh
sudo nano /etc/apt/preferences.d/nosnap.pref
```
> Package: snapd
> Pin: release a=*
> Pin-Priority: -10

then

```sh
apt update
```

## 2. Install Chromium and Firefox without snap

Enable XtraDeb repo

```sh
sudo add-apt-repository ppa:xtradeb/apps
sudo apt update
```
Install Chromium
```sh
sudo apt install chromium
```
install Firefox
```sh
sudo apt install firefox
```

## 3. Install Spotify

The offical package is absolutely disgustinig. Best way:

Open Spotify Web in Chrome `Options > More Tools > Create Shortcut...`

## 3. Electron apps and Chrome on Wayland

**VScode**
```sh
cp /usr/share/applications/code.desktop ~/.local/share/applications/code.desktop
```
```sh
nano ~/.local/share/applications/code.desktop
```

edit exec like this:
> Exec=/usr/share/code/code --unity-launch --enable-features=WaylandWindowDecorations --ozone-platform-hint=auto --disable-features=WaylandFractionalScaleV1 %F

**Chrome**

Open [chrome://flags](chrome://flags) and set `Preferred Ozone platform` to `Wayland`, then

```sh
cp /usr/share/applications/chromium.desktop ~/.local/share/applications/chromium.desktop
```
```sh
nano ~/.local/share/applications/chromium.desktop
```
edit exec like this:
> Exec=/usr/bin/chromium %U --disable-features=WaylandFractionalScaleV1


**Fix windows decoration and cursor theme for Electron wayland apps**
```sh
sudo apt-get install gnome-tweak-tool
```

Open tweak tool `open Appereance > Cursor > set any theme and revert default`, then `open Window Titlebars > set Placement Left > set Placement Right`

This will make the applications look similar to the entire system.


## 4. Sleep fix for Logi USB Unifying Receiver

The laptop may wake up from sleep mode almost immediately, here's how to fix it:
```sh
sudo apt-get install solaar && sudo apt-get install git && git clone https://github.com/3v1n0/Solaar.git ~/solaar && cd ~/solaar/rules.d && ./install.sh
```
## 5. Change the input switching shortcut to alt+shift
```sh
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Shift>Alt_L']"
```
```sh
gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Alt>Shift_L']"
```

## 6. Disable hibernate
```sh
systemctl mask hibernate.target hybrid-sleep.target
```

## 7. Limit battery charging to 80%

set limit
```sh
echo "79" | sudo tee /sys/class/power_supply/BAT0/charge_control_start_threshold
echo "80" | sudo tee /sys/class/power_supply/BAT0/charge_control_end_threshold

```

reset limit
```sh
echo "100" | sudo tee /sys/class/power_supply/BAT0/charge_control_end_threshold
echo "99" | sudo tee /sys/class/power_supply/BAT0/charge_control_start_threshold
```

## 8. Better touchpad support for thinkpad t14 g2

add to kernel parameters in `/etc/default/grub` `psmouse.synaptics_intertouch=1`
then
```sh
sudo update-grub
```



## P.S.

more about electron option for wayland

**--ozone-platform-hint=auto**

Enabled wayland support

**--enable-features=WaylandWindowDecorations**

By default, electron apps draw their window decorations (or don't do so at all), so this option will enable support for native decorations

**--disable-features=WaylandFractionalScaleV1**

When using fractional scaling, Electron applications may show graphical artifacts in window decorations, this option helps to fix it. This may appear as small lines of a few pixels with glitches or desktop wallpaper parts. 


