# Passfile
Password manager, form filling software.

## Known issues

If the typed password (./passfile.py <pass>) is different to the printed password (./passfile.py -p <pass>), then you may be have the known bug on xcb https://gitlab.freedesktop.org/xorg/xserver/issues/257
Please try to set your X11 keyboard map with the setxkbmap command.
You can make it permament by adding the setxkbmap command to your .profile or .bashrc, or by adding the KB mapping in your Xorg.conf.

## Requirements

### System requirements
- Linux or BSD
- Python 3.7
- coreutils
- pip3
- libxdo3
- libx11-6
- a keyring supported by the python module keyring

### Python requirements
- pycryptodome
- PyYAML
- keyring

### Python recommends:
- python-libxdo

## CLI usage
```bash
pass --help
pass -i # initialize the passfile (ask for the encryption key)
pass -e # edit the passfile, you can store your passwords in YAML format
pass <pass_name> # write the password for you
pass -p # print the passfile content in plain text
```

## Passfile format
```yaml
<name>: <data to write as string>
```

```yaml
<name>:
 - <data to write as string>
 - type: <data to write as string>
 - key: <key to press> (can be Return, Tab...)
 - wait: <milliseconds to wait>
 - delay: <delay between each character to write (is applied until the next delay change or the end)>
```

### Example: 
```yaml
pass_name: 'my_password'
test:
 - "#bonjour"
 - wait: 2000
 - delay: 1000
 - ' monde'
 - key: Return
 - delay: 0
 - '#I am the '
 - key: Tab
 - pass file
 - key: Return
```

## Security
The passfile is encrypted in ChaCha20.
The encryption key is salted, derived with scrypt and stored in a password locked keyring supported by the python keyring module.

## Generating packages

### Debian package
You can generated a Debian package:

```bash
# git clone https://github.com/whuji/passfile.git
# cd passfile
# mkdir build
# cd build
# cmake .. && cpack
# apt install ./passfile_*.deb
```

### Arch package
```bash
# wget https://raw.githubusercontent.com/whuji/passfile/master/PKGBUILD
# makepkg -c
# pacman -U passfile-git.*.tar.xz
```

### BSD package
You can generate a BSD package:

```bash
# pip3 install pycryptodomex
# git clone https://github.com/whuji/passfile.git
# cd passfile
# mkdir build
# cd build
# cmake .. && cpack -G PKG
# pkg_add  ./passfile_*.pkg
```
