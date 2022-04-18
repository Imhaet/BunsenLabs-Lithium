## FIRST THING FIRST

This installation list is based on my preferences while trying out Bunsen Labs - Lithium (August, 2020) :fire: based on Debian10 and with Openbox  window manager (Installed w/ *lithium-3-amd64.hybrid* ISO) on my - quite old - macbook and is not a definitive procedure. Feel free to use this guide at your own peril.

<br />

### Settings and Partitions

:computer: Apple MacBook "Core 2 Duo" 2.0 13" (Unibody) Late 2008 Aluminum AKA MacBook5,1

Intel Core2 Duo P7350 @ 2x 1.995Ghz | amd64 EFI | 4GB RAM | NVIDIA GeForce 9400M @ 256MB

| Partition | File System | Mount     | Size              | Flags    |
| ---       | ---         | ---       | ---               | ---      |
| sda1      | fat32       | /boot/efi | 142MiB            | boot,esp |
| sda3      | ext4        | /         | 141.28GiB         |          |
| sda2      | linuxswap   |           | 7.63GiB (7812MiB) | swap     |

UEFI Boot with GUID Partition Table

### Important Installation Notes
* I had to boot on the live USB and use GParted to set up the the partition table before the installation.
* I had to use my phone as a WiFi hotspot to finish the installation because the process was getting stuck. It looks there is a problem with the installation when trying to set up IPv6 and it does not continue with my router. Found the solution in [this](https://forums.bunsenlabs.org/viewtopic.php?id=6745&p=2) forum.

From this point on `:$` means instructions in the command line and `:#` means that the instuctions should be run as `root` or with `sudo`.

---

<br />

## WHILE IT'S STILL FRESH

<br />

- [x] **Bunsen Labs WELCOME! script**
* You can use the **WELCOME!** script to do some updates on your system. It will first run `apt-get update` and `apt-get upgrade --with-new-pkgs`.

<br />

- [x] **Broadcom Wireless**

Worked out of the box! :smile:

<br />

- [x] **Enable Tapping and Reverse Scrolling (Natural) on Touchpad**
* While Tappinng worked out of the box, double-finger tap did not, but this is an easy fix by adding `TapButton2=3` into the autostart file.
* I also want to Reverse Scrolling (Natural) which will mean adding two parameters in a line on a file.
* First run the following command and take note of the Vertical Scroll value which will be multiplied by -1 when we edit the file:
```
:$ synclient | grep Delta
VertScrollDelta     = 235
HorizScrollDelta    = 235
CircScrollDelta     = 0.1
```
* Open the `~/.config/bunsen/autostart` file with your favorite editor like `nano`.
* Go to the `### TOUCHPAD: ` section and edit the line as follows:
```
synclient VertEdgeScroll=1 HorizEdgeScroll=1 TapButton1=1 TapButton2=3 VertScrollDelta=-235 2>/dev/null
```
* Log out and back in, it should work now.

<br />

- [x] **Sound (For MacBook Aluminum - late 2008)** :speaker:

Worked out of the box! :smile:

<br />

- [x] **Suspend/Sleep** :zzz:

Worked out of the box! :smile:

---

<br />

## TERMINAL STUFF

<br />

- [x] **Enable and Start Firewall**
```
:# apt install ufw
:# ufw default deny incoming
:# ufw default allow outgoing
:# ufw allow ssh
```
* If more ports need to be open (eg. port 80 for web server), use the comand `:# ufw allow` and then the *Port ##*. Finally, enable UFW (Uncomplicated FireWall).
```
:# ufw enable
:# ufw status
```

<br />

### Installing Apps

- [x] ~~**GParted**~~ *Lithium comes with GParted installed*
```
:# apt install gparted
```

- [x] ~~**GIT**~~ *Comes with the Debian 11 KDE install*
```
:# apt install git
```
* Configure git with:
```
:$ git config --global user.name "John Doe"
:$ git config --global user.email johndoe@example.com
:$ git config --global color.ui auto
```

- [x] ~~**VLC**~~ *Lithium comes with VLC installed*
```
:# apt install vlc
```

- [x] ~~**Firefox**~~ *Lithium comes with Firefox installed*
```
:# apt install firefox
```

- [x] **Neofetch**
```
:# apt install neofetch
```

---
# NOT COMPLETE!!!

<br />

## FINISHING (OPTIONAL) TOUCHES

- [x] **LaTeX**
* To avoid future problems with extra packages, install the full version of Tex Live. If other package installation problems should appear, this [thread](https://forums.linuxmint.com/viewtopic.php?t=300053) might help.
```
:# apt install texlive-full -y
:# apt install latexmk
```

<br />

- [x] **Atom** :link: [atom.io](https://atom.io)

To install Atom on Debian now that apt-key is deprecated, and have to manage keyring files in tusted.gpg.d instead, the following intructions are a bit different than the ones on Atom's webpage.

* First create a new directory to store local keys, it is important to separate them from the keys trusted by apt (ie. /etc/apt/trusted.gpg.d). Then download current key in base64 format (ascii-armored).
```
:# mkdir /etc/apt/local.trusted.gpg.d
:# wget -qO /etc/apt/local.trusted.gpg.d/atom-archive-keyring.asc https://packagecloud.io/AtomEditor/atom/gpgkey
```
* Export the armored key to a binary gpg file format:
```
:$ cat /etc/apt/local.trusted.gpg.d/atom-archive-keyring.asc | sudo gpg --dearmor --output /etc/apt/local.trusted.gpg.d/atom-archive-keyring.gpg
```
* Add the credentials to the sources.list; this is similar as the previous instructions, except for the new option signed-by which references the key:
```
:# sh -c 'echo "deb [arch=amd64 signed-by=/etc/apt/local.trusted.gpg.d/atom-archive-keyring.gpg] https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list'
```
* Run `apt update` for the changes to take effect, and then install Atom. Make sure you have the correct path (e.g., `~/Github`) for your *Project Home*.
```
:# apt update
:# apt install atom
```
* Some packages to consider installing would be:
  - [atom-updater-linux by andyrichardson](https://atom.io/packages/atom-updater-linux) - Checks automatically for updates when opening Atom.
  - [script by atom-community](https://atom.io/packages/script) - Run code in Atom!
  - [minimap by atom-minimap](https://atom.io/packages/minimap) - Displays a preview of the full source code.
  - [linter by steelbrain](https://atom.io/packages/linter) - A Base Linter with Cow Powers.
  - [linter-flake8 by AtomLinter](https://atom.io/packages/linter-flake8) - Atom linter plugin for Python, using flake8.
    * Needs to install flake8: `pip install flake8`
  - [python-autopep8 by markbaas](https://atom.io/packages/python-autopep8) - Format python code using autopep8.
    * Needs to install autopep8 `pip install autopep8`
  - [autocomplete-python by autocomplete-python](https://atom.io/packages/autocomplete-python) - Python completions for packages, variables, methods, functions, with their arguments. Powered by Kite.
  - [atom-python-virtualenv by pghilardi](https://atom.io/packages/atom-python-virtualenv) - To run our Python scripts with a virtual environment.
    * You have to add the sources every time you create a new virtual environment. For example, add `~/Github/NewProject;` to the *Additional virtualenvs*.
  - [file-icons by file-icons](https://atom.io/packages/file-icons) - Assign file extension icons and colours for improved visual grepping.

<br />

- [x] **LaTeX for Atom** :link: [atom.io](https://atom.io)
* For the Syntax highlighting and snippets, get [`language-latex` by area](https://atom.io/packages/language-latex).
* To compile LaTeX files from within Atom, install [`latex` by thomasjo](https://atom.io/packages/latex). Make sure *Enable SyncTeX* is selected and make `build` your *Output Directory* (You can also tick *Build on Save* and will do that every time you save).
* To show a document tree view for LaTeX, use [`latex-tree` by raphael-cch](https://atom.io/packages/latex-tree). Then go *Packages -> Latex Tree -> Toggle Tree View* to activate.
* To autocomplete citations, environments and references, the best package I've found is [`latexer` by Focus](https://atom.io/packages/latexer).
* To display the generated PDF in Atom you need [`pdf-view` by izuzak](https://atom.io/packages/pdf-view), then make sure that *Auto reload on update* is enabled.
* To spell check your LaTeX file, use the Core Package `spell-check`. Just add `text.tex.latex` into the *Settings -> Grammars*.
* ~~For LaTeX function autocompletion, [`latex-autocomplete`](https://atom.io/packages/latex-autocomplete).~~
* ~~Apparently the only LaTeX sensitive wordcount package is Aerijo's [`latex-wordcount`](https://atom.io/packages/latex-wordcount). It's still incomplete, but it does provide a couple of counting techniques.~~
* ~~For an undistracted writing experience check out the [Typewriter](https://atom.io/themes/pen-paper-coffee-syntax) theme.~~

- [x] **Atom NOT connecting to GitHub**
I am having an error when trying to clone or fetch data from GitHub, the error reads as '*git clone --recursive https:... ... fatal: protocol error: bad line length 2*'.
* The workarround that has somehow worked is as follows, and is based on this [thread](https://github.com/atom/atom/discussions/22668):
  1. On Debian, Atom's git resources are installed in two places, first go to `/usr/share/atom/resources/app.asar.unpacked/node_modules/dugite/git/bin/`.
  2. Rename the current's folder git, for example `:# mv git git.old`.
  3. Create a symbolic link to the regular git with `:# ln -s /usr/bin/git git`. If you `ls -l` you will see the new symbolic link as `git -> /usr/bin/git`.
  4. Do the same for `.../git/libexec/git-core/`.
  5. Clone your repository using the terminal `:$ git clone <url>` on your project directory.
  6. In Atom, open the folder with your cloned repository, everything should work now.

<br />

- [x] **Jupyter Notebook** :link: [jupyter](https://jupyter.readthedocs.io/en/latest/index.html)
Before installing Jupyter or any other python package, you may want to consider doing the instalation in a virtual environment. To do this, we first need to install python VE package with `:# apt install python3-venv` (We are using here Python 3). After installation you can then, move to your Python project folders and create a folder called `venv/` (For example `~/Github/TestRepo/venv/`). Now, on `~/Github/TestRepo/` run `source ./venv/bin/activate` and you will notice that the prommt has changed, it now starts with `(vevn) user@machine:~$`. Now you can install any package you want and will be installed for only that Project.
* First make sure you have [pip](https://jupyter.readthedocs.io/en/latest/glossary.html#term-pip) for Python installed. If not, just run `:# apt install python3-pip` and then run the command `pip3 -V` to verify the installation.
* Then install the Jupyter Notebook using:
```
pip3 install notebook
```
* Congratulations, you have installed Jupyter Notebook. To run the notebook:
```
jupyter notebook
```
* If you receive an error message such as `bash: jupyter: command not found` then it is possible that `~/.local/bin` is not on your path. Fix this by running `export PATH=$PATH:~/.local/bin` for your current session, then either log out and in or run `source ~/.bashrc`, then run `jupyter notebook` and it should open in your browser.

<br />

- [x] **Zotero** :link: [zotero.org](https://zotero.org/download/)
* Go to [zotero.org](https://zotero.org/download/) and `Download` the tarball file for Linux 64-bit.
* Extract the contents of the `Zotero-...x86_64.tar.bz2` tarball and move all the files into a new directory `/opt/zotero`.
* Run  the `/opt/zotero/set_launcher_icon` script from a terminal with `./set_launcher_icon` to update the .desktop file for that location.
* Symlink `zotero.desktop` for Zotero to appear in the launcher with `ln -s /opt/zotero/zotero.desktop /usr/share/applications/zotero.desktop`.
* Edit said file to point to the correct script:
```
:# nano /usr/share/applications/zotero.desktop
```
* Edit a few lines to the following and save :floppy_disk: :
```
[Desktop Entry]
Name=Zotero
Comment=Open-source Reference Management Tool (Stand Alone)
Exec=/opt/zotero/zotero
Icon=/opt/zotero/chrome/icons/default/default256.phg
Type=Application
Terminal=false
Categories=Office;
MimeType=text/plain
```
* After rebooting, the application should run from the menu.

---

<br />

## FINAL APP CLEANING

<br />

- [x] **Uninstall Apps**
* Uninstall non needed apps using the `sudo apt-get purge PROGRAM` command.

- [x] **Clean Up OS**
* After uninstalling, make sure there are no dependencies left by running the following commands:
```
:# apt autoremove
:# apt autoclean
```

<br />
:tada: THE END :tada:

<br />
<br />

---

<br />
<br />

## PERSONAL SYSTEM SETTINGS IN KDE

Following are my personal settings for the DE in KDE.

<br />

- [X] **Appearance**
* *Global Theme* -> Select **Breeze Dark** for the Theme
* *Plasma Style* -> Select **Layan plasma theme** for the Style
* *Application Style* -> Select **Breeze** for the Style
* *Application Style | Window Decorations* -> Remove the **Context help** from the Titlebar
* *Colors* -> Select **Breeze Dark** for the Colors
* *Icons* -> Select **Papirus-Dark** for the Icons
  * There is a script to change folder colors automatically that can be installed with:
  ```
  wget -qO- https://git.io/papirus-folders-install | sh
  ```
  * To show the current color and available colors for Papirus-Dark
  ```
  papirus-folders -l --theme Papirus-Dark
  ```
  * To change color of folders to blue-gray for Papyrus-Dark
  ```
  papirus-folders -C bluegrey --theme Papirus-Dark
  ```
  * Revert to default color of foldersfor Papirus-dark
  ```
  papirus-folders -D --theme Papirus-Dark
  ```
* *Cursors* -> Select **Vimix-cursors** or **Nordic-cursors** for the Cursors

- [X] **Workspace**
* **Workspace Behavior**
  * *General Behaviour* -> Click behavior: Select **Double-click to open files and folders**
  * *Screen Locking* -> Lock screen automatically: After **15 minutes**
  * *Screen Locking* -> Click Appearance: **Configure...** to select your Wallpaper
  * *Virtual Desktops* -> Add 2 Rows, **TOP** and **DOWN**
  * *Activities | Activities* -> Add new activity called **Work**
  * *Activities | Switching* -> `Walk through activities` set the **Global Alternate** shortcut to **Meta+Ctrl+Right**
  * *Activities | Switching* -> `Walk through activities (Reverse)` set the **Global Alternate** shortcut to **Meta+Ctrl+Left**
* **Shortcuts**
  * *Shortcuts | KWin* -> `Quick Tile Window to the Bottom` **Uncheck** Default shortcut
  * *Shortcuts | KWin* -> `Quick Tile Window to the Top` **Uncheck** Default shortcut
  * *Shortcuts | KWin* -> `Maximize Window` **Uncheck** Default shortcut and add **Meta+Up**
  * *Shortcuts | KWin* -> `Minimize Window` **Uncheck** Default shortcut and add **Meta+Down**
  * *Shortcuts | KWin* -> `Switch One Desktop Down` **Check** Default shortcut **Meta+Ctrl+Down**
  * *Shortcuts | KWin* -> `Switch One Desktop Up` **Check** Default shortcut **Meta+Ctrl+Up**
* **Startup and Shutdown**
  * *Login Screen (SDDM)* -> Select the **Chili for Plasma** if it hasn't been selected, and change the background image
  * *Desktop Session* -> `On Login` section: Select the **Start with an empty session**
  * *Splash Screen* -> Install and select **YAPLASS Debian** because of course

- [X] **Personalization**
* **Applications**
  * *Default Applications* -> for Web Browser: select **Falkon**
  * *Launch Feedback* -> for Cursor: select **No Feedback**

- [X] **Hardware**
* **Input Devices**
  * *Touchpad | Tapping:* -> **Check** Tap-to-click
  * *Touchpad | Scrolling:* -> **Check** Invert scroll direction (Natural scrolling)
* **Display and Monitor**
  * *Compositor* -> for Rendering backend: select **XRender** for the Nvidia card
* **Power Management**
  * *Energy Saving | On AC Power* -> `Dim screen` after **10 min**
  * *Energy Saving | On AC Power* -> `Screen Energy Saving` After **30 min**
  * *Energy Saving | On AC Power* -> `Button events handling` `When laptop lid is closed` **Turn off screen**
  * *Energy Saving | On Battery* -> `Dim screen` after **2 min**
  * *Energy Saving | On Battery* -> `Screen Energy Saving` Switch off after **5 min**
  * *Energy Saving | On Battery* -> `Suspend session` Automatically **Shut down** after **15 min**
  * *Energy Saving | On Battery* -> `Button events handling` `When laptop lid is closed` **Shut down**
  * *Energy Saving | On Low Battery* -> `Dim screen` after **1 min**
  * *Energy Saving | On Low Battery* -> `Screen Energy Saving` Switch off after **2 min**
  * *Energy Saving | On Low Battery* -> `Suspend session` Automatically **Shut down** after **5 min**
  * *Energy Saving | On Low Battery* -> `Button events handling` `When laptop lid is closed` **Shut down**
* **Bluetooth**
  * **Disable Bluetooth** since I'm not using it at the moment

<br />

### Panel Preferences

- [x] **Panel**
* *Panel height:* -> Set to **30**
* **Icons-only Task Manager**
  * **Application Launcher**
    * *Show Alternatives...* -> Select **Application Dashboard** instead of the Launcher
    * *Configure Application Dashboard... -> On `Keyboard Shortcuts` add custom shortcut **Meta+Space**
  * *Leave **Pager Widget** as is*
  * *No launch icons*
  * **System Tray**
    * *Entries* -> set `IBus Panel` Visibility to **Always hidden**
    * *Entries* -> set `Battery and Brightness` Visibility to **Always shown**
    * *Entries* -> set `Clipboard` Visibility to **Always hidden** (This because it basically saves everything you type!)
  * **Digital Clock**
    * *Appearance* -> **Uncheck** ` Show date
    * *Appearance* -> set `Time display:` to **24-Hour**

<br />

### Dolphin Preferences

- [x] **Left Panel**
* *Places* -> Hide Section **Devices**

- [x] **Menubar**
* *View* -> **Uncheck** Show Previews (On the main window, you can resize the folder icons back to 64px)
* *View | Show Panels* -> **Uncheck** show Places

- [x] **Toolbar Settings**
* **Uncheck** Lock Toolbar Positions
* *Text Position* -> Icons Only
* *Orientation* -> Left
* *Configure Toolbars...*
  * **Current actions**
    * *Back*
    * *Forward*
    * *--- separator ---*
    * *Home*
    * *Icons*
    * *Compact*
    * *Details*
    * *Places*
    * *Show Previews*
    * *Split*
    * *Information*
    * *Search*
    * *Terminal*

<br />

### Konsole Terminal Preferences

- [x] **General**
* *Process and window:* -> **Uncheck** Show menubar
* *Process and window:* -> **Check** Remove window titlebar and frame

- [x] **Profiles**
On a new Profile...
* *General* -> Give this profile a *New Name*
* *General* -> Set the **Initial terminal size:** to **120 columns** and **40 rows**
* *Appearance* -> Select **Linux Colors**
* *Appearance | Edit...* -> Set **Background transparency:** to **30%**
* Select the New profile **Set as Default**

<br />
:octocat:
