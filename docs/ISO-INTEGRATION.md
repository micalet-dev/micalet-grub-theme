# Integrating the theme into the Micalet live ISO

The Debian package configures GRUB on an installed system. The live ISO has
its own GRUB files, so copy the theme into the ISO's boot tree as well:

```bash
ISO="$HOME/micalet/iso"
THEME="$HOME/micalet/packages/micalet-grub-theme/theme"
DEST="$ISO/binary/boot/grub/themes/micalet"

sudo install -d "$DEST"
sudo install -m 644 "$THEME"/* "$DEST/"
```

In `config/grub/grub.cfg`, retain the current four Micalet menu entries and
load the theme before them:

```grub
insmod all_video
insmod gfxterm
insmod png
insmod font
set gfxmode=auto
loadfont $prefix/fonts/unicode.pf2
set theme=$prefix/themes/micalet/theme.txt
terminal_output gfxterm
```

Ensure `binary/boot/grub/fonts/unicode.pf2` exists. Regenerate the ISO with
the GRUB rescue tooling already used by the project, and test in VirtualBox
in both BIOS and UEFI modes before publishing a stable release.
