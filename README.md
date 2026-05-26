# compact-top-plus

A small fork of Zellij's built-in `compact-bar` plugin for a cleaner top bar.

It keeps the original compact-bar keybind helper behavior while changing the visible bar:

- Shows `[session-name]` on the left instead of `Zellij (session-name)`.
- Shows only `LOCKED` or `NORMAL`; pane/tab/resize/etc modes are displayed as `NORMAL`.
- Shows Moscow time (`UTC+03:00`) on the right.
- Hides the permanent `F1 Tooltip` indicator from the bar.
- Keeps the popup keybind helper when the configured tooltip shortcut is pressed.

## Requirements

- Zellij `0.44.3`.
- Rust with the `wasm32-wasip1` target if building from source.

```sh
rustup target add wasm32-wasip1
```

## Install From Source

```sh
git clone git@github.com:ivanesmantovich/compact-top-plus.git
cd compact-top-plus
cargo build --release --target wasm32-wasip1
mkdir -p ~/.config/zellij/plugins
cp target/wasm32-wasip1/release/compact-top-plus.wasm ~/.config/zellij/plugins/compact-top-plus.wasm
```

## Install Prebuilt Wasm From This Repo

If you trust the checked-in wasm, copy it directly:

```sh
mkdir -p ~/.config/zellij/plugins
cp compact-top-plus.wasm ~/.config/zellij/plugins/compact-top-plus.wasm
```

## Zellij Configuration

Add or replace the `compact-bar` alias in `~/.config/zellij/config.kdl`:

```kdl
plugins {
    compact-bar location="file:/home/YOUR_USER/.config/zellij/plugins/compact-top-plus.wasm" {
        tooltip "F1"
    }
}
```

Keep using a compact top layout that loads `compact-bar`:

```kdl
layout {
    pane size=1 borderless=true {
        plugin location="compact-bar"
    }
    pane
}
```

Restart Zellij after changing plugin aliases.

## Permissions

Because this is loaded from `file:` rather than as Zellij's built-in plugin, Zellij will request permissions on first launch. Grant these:

- `ReadApplicationState`
- `ChangeApplicationState`
- `MessageAndLaunchOtherPlugins`
- `Reconfigure`

They are needed to read tabs/modes, register the tooltip shortcut, and launch/update the keybind helper popup.

## Notes

The visible `F1 Tooltip` indicator is intentionally hidden, but the `tooltip "F1"` setting must remain. It is still used internally to bind the helper shortcut.

## License

MIT, same as Zellij.
