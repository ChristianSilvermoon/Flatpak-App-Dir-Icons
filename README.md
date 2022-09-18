# Flatpak AppDir Icons

If you use Flatpak, you're probably familiar with the `~/.var/app` directory where your Flatpak apps' data goes...

If you've ever browsed this directory with your GUI file manager, and you have *many* apps, you might notice that
it's kind of a *sea* of folders with strange `tld.domain.App` style folder names.

You cannot rename these, it's important they stay as they are!

But Humans are GOOD at recognizing **Icons**. It's why we use them so prevalently.

So, in an effort to make `~/.var/app` more pleasant to browse... a simple shell script to give you *Icons*.

This also helps you see **RIGHT AWAY** which of your Flatpak App Directories are still useful and which belong to apps you
no longer have installed and/or no longer need.

## How Does It Work?

This script works by doing the following.
1. Analyzing the list of directories in `~/.var/app`
2. Checking for Exported Icons & Binaries in `/var/lib/flatpak/exports` and `$XDG_DATA_HOME/flatpak/exports`
3. Telling your OS what the icons/emblems for the respective folder in `~/.var/app` should be.

For desktops relying on `gio`/`gvfs` such as GNOME, Cinnamon, etc.
It will set the icon and emblem of the file as if you did so yourself by calling `gio` to manipulate metadata.

For KDE Plasma, it will create a `.directory` entry specifiying the icon within the app's folder.

This all results in the following
* App Directories are set to their respective app icons if possible.
* If the app is uninstalled, it is given the `emblem-important` emblem (or its icon set to `folder-red` in KDE's case since it doesn't support emblems)
* If the app is installed, it is given the `emblem-installed` emblem (nothing in KDE's case.)
* If there is a binary, but no Icon, this script assumes it is a command-line program such as [ponysay](https://github.com/erkin/ponysay), which is available through Flatpak and will set the folder icon to `terminal` (`utilities-terminal` for KDE)

...thus making your App Directories far more pleasant to browse!

You could set this up as a cronjob and forget about it :P
```crontab
# Update app folder icons in ~/.var/app
*/30 * * * *  flatpak-app-dir-icons
```
In this case, the job would run every half hour.

![image](./nemo-example.png)
