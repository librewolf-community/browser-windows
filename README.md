# LibreWolf for windows

* **[download latest release](https://gitlab.com/librewolf-community/browser/windows/-/releases)**
* Visit [the FAQ](https://librewolf.net/docs/faq/).
* Install via _[chocolatey](https://community.chocolatey.org/packages/librewolf)_: `choco install librewolf`
* or install via _[scoop](https://scoop.sh)_: `scoop bucket add extras`, then `scoop install librewolf`
* or install via _winget_: `winget install librewolf`
* **If your LibreWolf crashes on startup**, you're probably missing the right [Visual C++ Runtime](https://support.microsoft.com/en-us/topic/the-latest-supported-visual-c-downloads-2647da03-1eea-4433-9aff-95f26a218cc0). You'll want the _Visual Studio 2015, 2017 and 2019_ version for **x64**, which would be **[this file](https://aka.ms/vs/16/release/vc_redist.x64.exe)**.
* The latest type of **.zip files** allows for a user profile inside the extracted folder. It is _self-contained_ and runs on removable storage.

# Help wanted

If you want to contribute, we are moving this repository from building natively from windows 11 to cross-build from Linux/Docker in our cool gitlab CI[[here](https://gitlab.com/librewolf-community/browser/bsys5/-/issues/12)]. At the same time, we want to make auto-update work[[here](https://gitlab.com/librewolf-community/browser/windows/-/issues/237)]. People want it, and on Windows I guess it's the easiest option.

Thanks!

# Where to submit tickets

* For all **about:config** and **librewolf.cfg** issues, go here: [[settings repository](https://gitlab.com/librewolf-community/settings/-/issues)].
* For _all other issues_ and **setup/install** issues, go here: [[issues for windows repository](https://gitlab.com/librewolf-community/browser/windows/-/issues)].

# Community links
* [[reddit](https://www.reddit.com/r/LibreWolf/)] - [r/LibreWolf](https://www.reddit.com/r/LibreWolf/) 😺
* [[gitter](https://gitter.im/librewolf-community/librewolf)], and the same room on [matrix](https://app.element.io/#/room/#librewolf-community_librewolf:gitter.im) (element.io).
* The install instructions for Windows on [librewolf.net](https://librewolf.net/installation/windows/).

# Community contributions

* Defkev created a LibreWolf updater plugin, which can be found [here](https://addons.mozilla.org/en-US/firefox/addon/librewolf-updater/).
* Guillaume created a windows updater script for the Task Scheduler. it can be found [here](https://github.com/ltGuillaume/LibreWolf-WinUpdater).

# Compiling the windows version

This segment is for people who want to build LibreWolf for themselves. The build of the LibreWolf source tarball is in public CI, so you can use that. Given that you have followed the steps in the Mozilla setup guide:

* [Building Firefox On Windows](https://firefox-source-docs.mozilla.org/setup/windows_build.html)

Once that works, you can check out and compile LibreWolf like this:

```
git clone https://gitlab.com/librewolf-community/browser/windows.git
cd windows
make fetch build
```

Currently a bug in `./mach package` makes this build fail, but it did produce the distribution .zip file that we're after. So after this, you can just:

```
make artifacts
```
This will produce the -setup.exe and portable .zip. Have fun!

# Uploading a release

To actually submit these artifacts to the Windows repository as release files, use:
```
python3 mk.py upload <token>
```
This would involve having a valid token, ofcourse, but also something more: [Git for Windows](https://git-scm.com/). From this package, we only need `sha256sum.exe` to calculate our checksums. Mozilla provides only `md5sum.exe` in their very old version of the mingw tools. Simply installing Git won't be enough to get `sha256sum.exe` in our path, the `C:\mozilla-build\start-shell.bat` file needs a little tweak at line 55, to read:
```
SET "PATH=%PATH%;!GITDIR!;c:\Program Files\Git\usr\bin"
```
This should put `sha256sum.exe` in your path.
