# Important

If you are running a University administered machine, you will hopefully have pre-configured your machine.
The following installations will only be able to be performed if you have Administration privileges on your computer.
If you have not pre-configured your machine, we will provide you with an USB containing a live install of Ubuntu for you to use.

## Installing Bash on Windows

If you are running Windows 10 and have [already installed Ubuntu as an app](https://tutorials.ubuntu.com/tutorial/tutorial-ubuntu-on-windows#0), please use this installation in preference to the below.
Otherwise, please follow these instructions to install a working version of bash on your computer.

1. Download and install `git bash` by going to the following site: [https://git-for-windows.github.io/](https://git-for-windows.github.io/) and selecting the `Git-2.xx.x-(32|64)-bit.exe` file as is appropriate for your computer.
If you're unsure if you have a 32 or 64 bit computer, follow [these instructions](https://www.lifewire.com/am-i-running-a-32-bit-or-64-bit-version-of-windows-2624475)
2. When presented with this screen during installation, select the first option

![](https://blog.assembla.com/hs-fs/hub/365/file-2182891772-png/Blog/Git_on_windows_blog/Git_adjustPath.png?t=1505570223016)

- Accept defaults for all other options, especially the following:

![](https://blog.assembla.com/hs-fs/hub/365/file-2181997909-png/Blog/Git_on_windows_blog/Git_Configure_LineEndings.png?t=1505570223016)

## Installing `wget`

After installation, you will need to install the additional tool called `wget`.
To perform this installation

1. Download the [32-bit](https://eternallybored.org/misc/wget/1.19.4/32/wget.exe) or [64-bit](https://eternallybored.org/misc/wget/1.19.4/64/wget.exe) file ending in `.exe`
2. Move this file to `C:\Program Files\Git\mingw64\bin\`

Once you have completed the installation, open Git Bash and enter the following in the terminal

```
wget --version
```

If you receive an error message **use the post-it notes** to ask for help from an instructor

## Installing `nano`

One final tool we need to install is the `nano` editor.
We'll use this to edit text files directly in the terminal.
This install can be a bit tricky, so before you move on close `git bash` if you have it open, then re-open `git bash` using your right-click to select `Run as administrator`

From there on just paste the following commands:
```
wget https://www.nano-editor.org/dist/win32-support/nano-git-0d9a7347243.exe
mv nano-git-0d9a7347243.exe /c/Program\ Files/Git/usr/bin/nano.exe
echo 'alias nano="winpty nano"' >> ~/.bash_profile
```

Close and reopen `git bash` then enter `nano` in the terminal.
If you enter what looks like a *blank editor screen*, you've been sucsessful.
You can exit `nano` by entering `Ctrl+x`.
If you have not been successful, call an instructor over.

## Installing Notepad++

Although we may not use this much today, an excellent alternative editor for scripts on the Windows OS is *Notepad++*.
A key difference between Windows and Linux/OSX systems is the *hidden characters* used to denote `end-of-line` (EOL).
Most editors such as Wordpad, MS Word etc **will change these without you knowing** and scripts will not be able to be run if edited using these programs.
*Notepad++* does not do this and using this software will allow you to edit scripts conveniently without breaking them.

Notepad++ can be obtained from [here](https://notepad-plus-plus.org/download/v7.5.1.html)

[Home](../)
