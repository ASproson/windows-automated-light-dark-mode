# windows-automated-light-dark-mode

It's quite convoluted to set a scheduled light/dark mode on Windows, so here's a simple way to manage that:

> Search `Task Scheduler`

> Click `Task Scheduler Library`

> Click `New Folder...` and name it Night Mode (or whatever you like, this is where we store our schedule tasks)

> Click your new folder and then click `Create Task`

Remember, we need two tasks. One to switch to dark mode, and another to switch from dark mode back to light mode. We'll start with the switch to dark mode

## Dark Mode Automation

> Name the task `switchToDarkMode`

> In `General` select `Run whether user is logged on or not` and tick `Do not store password`

> Check the box to `Configure for Windows 10` (or whatever OS you're using)

> Click `Triggers`, make sure it's set to `On a schedule

> Check the box `Daily` and set your start date and tim

> Click `Actions` and make sure its set to `Start a program`

> In `Program/script` we need to paste in: `%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe`

> In `Add arguments` we need to paste in: `:New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name SystemUsesLightTheme -Value 0 -Type Dword -Force; New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 0 -Type Dword -Force`

> Click `Settings` and check `Run task as soon as possible after a scheduled start is missed` and `If the task fails, restart`. This just solves any sleep mode problems

## Light Mode Automation

> Name the task `switchToLightMode`

> In `General` select `Run whether user is logged on or not` and tick `Do not store password`

> Check the box to `Configure for Windows 10` (or whatever OS you're using)

> Click `Triggers`, make sure it's set to `On a schedule`

> Check the box `Daily` and set your start date and time

> Click `Actions` and make sure its set to `Start a program`

> In `Program/script` we need to paste in: `%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe`

> In `Add arguments` we need to paste in: `New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name SystemUsesLightTheme -Value 0 -Type Dword -Force; New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 1 -Type Dword -Force`

> Click `Settings` and check `Run task as soon as possible after a scheduled start is missed` and `If the task fails, restart`. This just solves any sleep mode problems

**NOTE**: This light mode automation sets the taskbar to dark mode, which is my preference. Alternatively if you want the taskbar to be light mode too you can paste this in the arguments instead:

> `New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name SystemUsesLightTheme -Value 1 -Type Dword -Force; New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 1 -Type Dword -Force`

## Extra Options

You might have specifc behaviour you prefer, such as hiding the task bar automatically and other things like that. It's likely that these scripts will overwrite those preferences. As such, I'd recommend asking ChatGPT to add those conditions to the `Add arguments` script, just be sure use to the exact lettering/wording used in the Windows Settings
