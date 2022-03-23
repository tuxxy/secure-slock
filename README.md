# secure-slock - a paranoid fork of suckless' slock

Inspired by [chjj's fork of slock](https://github.com/chjj/slock/)

## Changes from slock
- This fork of slock is based on the latest commit from 25 MAR 2017
    - [35633d45672d14bd798c478c45d1a17064701aa9](https://git.suckless.org/slock/commit/35633d45672d14bd798c478c45d1a17064701aa9.html)
    - I will keep it updated with the latest patches, when possible.
- Automatic shutdown - The screenlocked machine will automatically shutdown if:
    1. The wrong password is entered more than `UNLOCK_ATTEMPTS_MAX` times (by default 3), or
    2. ALT/CTRL/F1-F13 is pressed to switch VTs or to try to kill the X server. Also, if ALT+SYSRQ is attempted to be used.
    - To use this, add the following to your `/etc/sudoers` file:
        - ensure ACCESS\_COMMAND is set to "sudo -n"
        - `[username] [hostname] =NOPASSWD: /usr/bin/shutdown -h now`
            - Replace `[username]` and `[hostname]` with the username and hostname of your machine.
    - Or if you use doas, edit `/etc/doas.conf`:
        - ensure ACCESS\_COMMAND is set to "doas -n"
        - 'permit nopass [username] as root cmd shutdown'
            - Replace `[username]` with the username your machine.
- Process killing countermeasures - prevents an attacker from using Alt+sysrq and Ctrl+Alt+Backspace by disabling it before shutdown.
    - To use this, add the following to your `/etc/sudoers` file:
        - ensure ACCESS\_COMMAND is set to "sudo -n"
        - `[username] [hostname] =NOPASSWD: /usr/bin/tee /proc/sys/kernel/sysrq`
            - Replace `[username]` and `[hostname]` with the username and hostname of your machine.
    - Or if you use doas, edit `/etc/doas.conf`:
        - ensure ACCESS\_COMMAND is set to "doas -n"
        - 'permit nopass [username] as root cmd shutdown'
            - Replace `[username]` with the username your machine.
- Relicensed under the GPLv3
    - Any new changes to the source are licensed under GPL, the original slock code is licensed under the MIT license. It is also included in `MIT_LICENSE`.

## Installation
- Edit `config.mk` to match your local machine, if needed. Slock will be installed into the `/usr/local` namespace by default.
- Edit `config.def.h` with your desired changes.
    - Then rename it to `config.h`.
- Build slock (as root, if neccessary):
    ```
    $ make clean install
    ```

## Coming Soon
- BadUSB protection
- Alarms
- Custom lock screen
- Perhaps more?
