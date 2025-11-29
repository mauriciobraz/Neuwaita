<h1 align="center">Neuwaita</h1>
<h4 align="center">A different take on the Adwaita theme.</h4>

![Showcase](https://github.com/mauriciobraz/Neuwaita/blob/main/assets/Showcase.png)
![Mimes](https://github.com/mauriciobraz/Neuwaita/blob/main/assets/Mimes.png)

## Installation

### User installation
Clone the repository into `~/.local/share/icons/Neuwaita`:
```bash
git clone --depth 1 https://github.com/RusticBard/Neuwaita.git ~/.local/share/icons/Neuwaita
```

### System-wide installation
Clone the repository into `/usr/share/icons`:
```bash
sudo git clone --depth 1 https://github.com/RusticBard/Neuwaita.git /usr/share/icons/Neuwaita
```

## Updating

To update Neuwaita icon theme to the latest version:

```bash
# User installation
git -C ~/.local/share/icons/Neuwaita pull

# System-wide installation
sudo git -C /usr/share/icons/Neuwaita pull
```

## Customize folder colors

Run `change-color.sh` to change the folder's colors. See [Palette.txt](https://github.com/RusticBard/Neuwaita/blob/main/Palette.txt) for available colors.

```bash
# Change the folders to blue
./change-color.sh blue

# Reset the color to grey
./change-color.sh reset
```

## Setup automatically changing folder color when accent color is changed

*Requires systemd on GNOME (for now)*

Create a systemd service file:

```bash
mkdir -p ~/.config/systemd/user
nano ~/.config/systemd/user/watchAccent.service
```

Paste the following content inside the `watchAccent.service` file. **Don't forget to change `<username>`**:

```ini
[Unit]
Description=Neuwaita Accent Color Watcher
After=graphical-session.target

[Service]
ExecStart=/home/<username>/.local/share/icons/Neuwaita/watch-accent.sh
Restart=always
RestartSec=3
Environment=DISPLAY=:0
Environment=XDG_CURRENT_DESKTOP=GNOME

[Install]
WantedBy=default.target
```

After saving and exiting, run the following commands:

```bash
systemctl --user daemon-reload
systemctl --user enable watchAccent.service
systemctl --user start watchAccent.service
```

Logout and log back in to get it working!

## Requesting new icons

I understand you really want the icon, but when making an icon request:

1. Check [here](https://github.com/RusticBard/Neuwaita/issues/7#issue-1534235372) if the icon you want is already present or in the making.
2. Please include the [actual name of the icon](#how-to-find-the-actual-name-of-the-icon) that you want to request.

## Using a fallback theme

You can tell the system to use a fallback theme in case Neuwaita doesn't provide an icon for your app.

1. Navigate to Neuwaita installation directory (either `~/.local/share/icons/Neuwaita` or `/usr/share/icons/Neuwaita` depending on your installation)
2. Edit the `Inherits` variable in `index.theme`:

```ini
[Icon Theme]
Name=Neuwaita
Comment=Neuwaita icon theme
Inherits=theme-name,theme-name-2
Example=folder
```

You can add as many inherits as you wish. In the example above, icons will be first searched in `Neuwaita`, then `theme-name`, and then lastly in `theme-name-2`.

### How to find the **actual name** of the icon?

You're searching for the [reverse domain name notation](https://en.wikipedia.org/wiki/Reverse_domain_name_notation) (e.g., `org.mozilla.firefox` for Firefox). It can be found in different ways:

- Icons are usually located inside `/usr/share/icons/hicolor/scalable/<Name of your app>`
- For system-wide Flatpaks, the location is `/var/lib/flatpak/app/<Name of your app>` — the name of the folder is the name of your icon
- For user Flatpaks, the location is `~/.var/app/<Name of your app>`

If you don't want to find the icon in the files, you can also look for the app on Flathub. Go to the app you are requesting and scroll down to find installation instructions. There is a command you can copy as `flatpak install flathub <Actual name of your app>`.
