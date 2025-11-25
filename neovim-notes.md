# Neovim notes

## Setup
I followed this guide https://blog.nikfp.com/how-to-install-and-set-up-neovim-on-windows#heading-basic-requirements

It worked well but we can skip:
- The winget install
  * We already have this in modern Windows
- The Windows Terminal install
 * We already have this by default
- The PowerShell install
 * We don't need PowerShell core
- The article mentions that we have to run all the steps elevated to Administrator. This is simply not true


## Appearance
Per the nikfp guide I installed `AdwaitaMono` from Nerd Fonts and then set PowerShell to `AdwaitaMono Nerd Font Mono` in Terminal.

Larger font size. Right now using 15


## init.lua
Because we used the kickstart init.lua to set this up, we need to put our modifications in that file.

use `/MICHAEL` to search for the section where I add settings and also `[[` to find the various sections they encourage adding settings to. I added some under the `[[ Add settings here` section.

I have added so far:
### Changes for grepping
`:vimgrep` is the command to search multiple files
`:vimgrep maxmind **/*` searches all files for 'maxmind' for example

However, this would hit test data folders, "outputs" folders, things like .xlsx files, and also it would try to open .zip and throw errors because no unzip command is present.

We used the `wildignore` option to add folders to ignore.

And then defined a custom command 'V' to avoid having to constantly type out the aforementioned command. So we can just do:
`:V maxmind`


Then, because the results of commands like this are stored to quicklists, you have to run `:copen` and `:cclose` which again felt longwinded, so I've added so you can do `<leader>co` and `<leader>cc`. Our leader is the **space** key.

### Nerd Font settings
There is a setting at the top of the kickstart lua for if you have a nerd font. Not sure why but I set this to `true` as we are using one of those.


### Context menu
My nvim is located in 'C:\Program Files\Neovim\bin\nvim.exe' so I can run the following in an admin window:

```
# FILES - current user only
New-Item -Path 'Registry::HKCU\Software\Classes\*\shell\Open with Neovim' -Force | Out-Null
Set-ItemProperty -Path 'Registry::HKCU\Software\Classes\*\shell\Open with Neovim' -Name '(default)' -Value 'Open with Neovim'
Set-ItemProperty -Path 'Registry::HKCU\Software\Classes\*\shell\Open with Neovim' -Name 'Icon' -Value 'C:\Program Files\Neovim\bin\nvim.exe'

New-Item -Path 'Registry::HKCU\Software\Classes\*\shell\Open with Neovim\command' -Force | Out-Null
Set-ItemProperty -Path 'Registry::HKCU\Software\Classes\*\shell\Open with Neovim\command' -Name '(default)' -Value '"C:\Program Files\Neovim\bin\nvim.exe" "%1"'

# FOLDERS - current user only
New-Item -Path 'Registry::HKCU\Software\Classes\Directory\shell\Open with Neovim' -Force | Out-Null
Set-ItemProperty -Path 'Registry::HKCU\Software\Classes\Directory\shell\Open with Neovim' -Name '(default)' -Value 'Open with Neovim'
Set-ItemProperty -Path 'Registry::HKCU\Software\Classes\Directory\shell\Open with Neovim' -Name 'Icon' -Value 'C:\Program Files\Neovim\bin\nvim.exe'

New-Item -Path 'Registry::HKCU\Software\Classes\Directory\shell\Open with Neovim\command' -Force | Out-Null
Set-ItemProperty -Path 'Registry::HKCU\Software\Classes\Directory\shell\Open with Neovim\command' -Name '(default)' -Value '"C:\Program Files\Neovim\bin\nvim.exe" "%1"'

```
