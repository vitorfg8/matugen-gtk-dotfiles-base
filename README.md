# matugen-gtk-dotfiles — GTK3, GTK4, GNOME Shell & Ghostty driven by matugen

Material You colors for GTK3, GTK4/libadwaita apps, GNOME Shell and Ghostty,
generated from the wallpaper with [matugen](https://github.com/InioX/matugen).

* **GTK**: a Material surface ramp (sidebar lighter than content, highlighted
  cards/popovers, visible separators) instead of a single flat background.
* **GNOME Shell**: same selectors and structure as the
  [Rewaita](https://github.com/SwordPuffin/Rewaita) shell theme, with matugen
  colors: panel and app-launcher icon use `primary_fixed` (soft near-white tint
  of the accent), window borders off (transparent, like Rewaita with its
  window-borders option off), and squarish rounded quick-settings toggles
  (like Rewaita's no-pills option).
* **Ghostty**: terminal theme regenerated per wallpaper, reloaded
  automatically via `SIGUSR2` (set `theme = Matugen` in `ghostty/config`).

## Prerequisites

* [matugen](https://github.com/InioX/matugen) installed
* [Ghostty](https://ghostty.org/) terminal (for the Ghostty template)
* GNOME Shell with the **User Themes** extension enabled
* For GTK3: the [adw-gtk3](https://github.com/lassekongo83/adw-gtk3) theme
  installed (the GTK3 post-hook toggles back to `adw-gtk3-<mode>`)

## Install

```bash
# 1. Copy the files into place
cp -r matugen ~/.config/
cp -r themes/Material-Gnome ~/.local/share/themes/
cp ghostty/config ghostty/rounded.css ~/.config/ghostty/
chmod +x ~/.config/matugen/post-hook-scripts/gtk-themes-reload.sh

# 2. Wire the generated colors into GTK (matugen only writes colors.css;
#    GTK loads it through these imports). Add as the FIRST line of both files:
echo "@import 'colors.css';" | cat - ~/.config/gtk-3.0/gtk.css > /tmp/gtk3 && mv /tmp/gtk3 ~/.config/gtk-3.0/gtk.css
echo "@import 'colors.css';" | cat - ~/.config/gtk-4.0/gtk.css > /tmp/gtk4 && mv /tmp/gtk4 ~/.config/gtk-4.0/gtk.css

# 3. Preview without applying anything
matugen --dry-run --show-colors image ~/Pictures/your-wallpaper.jpg --source-color-index 0

# 4. Generate (writes colors + shell + ghostty theme, reloads via post_hooks)
matugen image ~/Pictures/your-wallpaper.jpg --source-color-index 0
```
The bundled `ghostty/config` already sets `theme = Matugen`; back up yours
before overwriting it.

Reopen GTK apps afterwards — GTK reads its CSS at startup. The Shell theme
reloads automatically through the `post_hook`.

## Notes

* This is a very basic base dotfiles: just the matugen wiring (config +
  templates + one post-hook script), the generated shell theme metadata, and
  a minimal Ghostty config. No window manager, keybindings, or extra
  tooling — extend it as you like.
* The bundled Ghostty config sets `window-decoration = none` (no titlebar).
  To move the window, hold **Super + left-click-drag** anywhere on it (GNOME
  default window-drag shortcut).
* To bring the titlebar/decorations back, change that line to:

```ini
window-decoration = auto
```

  (or delete the line — `auto` is Ghostty's default). The window then uses
  normal server-side decorations again.

## Layout

```
matugen/
  config.toml                 # [config] + wallpaper off + 4 template entries
  templates/
    gtk-colors.css            # Material surface ramp (GTK3 + GTK4)
    gnome-shell.css           # Rewaita-structured shell theme, matugen colors
    ghostty                   # Ghostty theme (→ ~/.config/ghostty/themes/Matugen)
  post-hook-scripts/
    gtk-themes-reload.sh      # toggles color-scheme so GTK4 re-reads CSS
themes/
  Material-Gnome/
    index.theme               # shell theme metadata (Adwaita icons/cursor)
ghostty/
  config                    # clean Ghostty config (theme = Matugen, no comments)
  rounded.css               # 12px window rounding (referenced by gtk-custom-css)
```

## Regenerating / rollback

* New wallpaper: `matugen image <file> --source-color-index 0` (or without the
  flag for an interactive source-color picker in a terminal).
* Rollback: back up your current `~/.config/gtk-3.0/gtk.css`,
  `~/.config/gtk-4.0/gtk.css`, `~/.config/ghostty/config` and dconf keys
  (`org.gnome.shell.extensions.user-theme name`,
  `org.gnome.desktop.interface gtk-theme`) before installing, then restore
  them.

## Credits & license

* Shell structure derived from
  [Rewaita](https://github.com/SwordPuffin/Rewaita)
  (`src/themes/gnome-shell-template.css`, no-pills CSS) — **GPL-3.0**.
* Template approach inspired by
  [matugen-themes](https://github.com/InioX/matugen-themes) (MIT) and the
  [matugen wiki](https://github.com/InioX/matugen/wiki).
* This repo as a whole is distributed under **GPL-3.0-or-later** (see
  `LICENSE`), following the strongest copyleft of its sources.
