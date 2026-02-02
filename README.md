# rfg — remote fish gossip 🐟

A tiny Linux CLI for passing small text snippets between machines using a shared filesystem.

`rfg` stores each snippet as a plain text file. If the storage directory is synchronised
(for example with Syncthing), the snippets appear on your other machines automatically.

Designed for Fish shell users, no networking code.

---

## Requirements

- Linux
- fish shell

For cross machine syncing (recommended):
- Syncthing, or any other directory synchronisation tool

Optional features:
- `fzf` for interactive picking
- `wl-clipboard`, `xclip`, or `xsel` for clipboard integration

---

## Install

Clone the repository:

```sh
git clone https://github.com/saysomestuff/rfg.git
cd rfg
```

Install for the current user:

```sh
mkdir -p ~/.local/bin
install -m 755 bin/rfg ~/.local/bin/rfg
```

Ensure `~/.local/bin` is on your PATH (Fish):

```fish
fish_add_path -U ~/.local/bin
exec fish
```

Verify installation:

```sh
rfg -h
```

---

## Configuration

Edit the config section at the top of the `rfg` script *or* set $RFG_SYNC_DIR locally:

```fish
set -g RFG_DIR "$HOME/.sync/rfg"
```

`RFG_DIR` should point to a directory inside a folder that is shared between machines.

Create it once:

```sh
mkdir -p ~/.sync/rfg
```

If you are using Syncthing, you should synchronise the parent folder (`~/.sync`),
not the `rfg` directory itself.

---

## Usage

Store text from stdin:

```sh
echo "hello world" | rfg -s a "example"
```

Store text with an inline body:

```sh
rfg -s note "quick note" "some text here"
```

List stored entries:

```sh
rfg -l
```

Read an entry:

```sh
rfg -r a
```

Pick an entry interactively (requires `fzf`):

```sh
rfg -p
```

Copy an entry to the system clipboard:

```sh
rfg -y a
```

Copy the most recent shared clipboard entry:

```sh
rfg -Y
```

---

## First test

On machine A:

```sh
echo "hello from A" | rfg -s test "sync test"
```

On machine B:

```sh
rfg -r test
```

If the text appears, everything is working.

---

## Notes

- `rfg` only reads and writes plain text files
- All syncing is handled by your sync tool
- Conflicts are resolved by the sync tool, not by `rfg`
- Temporary `.tmp` files may appear briefly during sync; this is normal

---

## License

MIT
