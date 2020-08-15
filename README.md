# qidle

qidled is an idle daemon for [Qubes OS](https://www.qubes-os.org/).

It can detect the user or VMs going idle and accordingly trigger user-defined actions (usually a VM shutdown).

Besides the obvious energy savings it is mostly intended to harden a Qubes OS host against memory [side-channel](https://en.wikipedia.org/wiki/Side-channel_attack) and other attacks originating from compromised VMs.

The basic idea is:
Halted Attacker VMs cannot attack and halted target VMs are harder to attack. So let's make an attacker's life harder by halting everything we don't need!

## Features

- not interruptable by (potentially malicious) VMs as it runs in dom0
- works with all Qubes OS VMs
- no input from VMs
- highly customizable (timeouts per VM, actions, enable/disable per VM, ...)

### Why another?

The [official Qubes OS solution](https://github.com/QubesOS/qubes-app-shutdown-idle) runs inside the respective VM which it is meant to monitor and halt as needed.

However if that VM is compromised, it can easily avoid its shutdown by e.g. re-writing the shutdown script. Afterwards it may use the time gained to deploy e.g. a memory side channel attack against other running VMs - all of this while the user is probably idle.

Moreover it only works on Fedora and Debian VMs and doesn't provide enough flexibility for my use cases.

Therefore I decided to write this script.

## Installation

1. Download [blib](https://github.com/3hhh/blib), copy it to dom0 and install it according to [its instructions](https://github.com/3hhh/blib#installation).
2. Download this repository with `git clone https://github.com/3hhh/qidle.git` or your browser and copy it to dom0.
3. Move the repository to a directory of your liking.
4. Symlink the `qidled` binary into your dom0 `PATH` for convenience, e.g. to `/usr/bin/`.
5. Configure `qidled` at `/etc/qidled.conf`. A sample configuration is included in your repository copy. Copy that one to `/etc/qidled.conf` for a start.
6. (Optional) Configure Qubes OS so that `qidled start` is run on autostart.

### A word of caution

It is recommended to apply standard operational security practices during installation such as:

- Github SSL certificate checks
- Check the GPG commit signatures using `git log --pretty="format:%h %G? %GK %aN  %s"`. All of them should be good (G) signatures coming from the same key `(1533 C122 5C1B 41AF C46B 33EB) EB03 A691 DB2F 0833` (assuming you trust that key).
- Code review

You're installing something to dom0 after all.

## Usage

Execute `qidled` on the command-line to obtain an overview of its capabilities.

The [sample configuration](https://github.com/3hhh/qidle/blob/master/qidled.conf) is a full-fledged bash file with lots of comments and should thus be mostly self-explanatory. Feel free to directly program your custom stuff inside your own configuration file. Happy hacking! :-)

## Uninstall

1. Remove all symlinks and autostart references that you created during the installation.
2. Remove the repository clone from dom0.
3. Uninstall [blib](https://github.com/3hhh/blib) according to [its instructions](https://github.com/3hhh/blib#uninstall).

## Copyright

© 2020 David Hobach
GPLv3

See `LICENSE` for details.
