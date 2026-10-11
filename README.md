# Parsec Switch

Manage several Parsec logins on one Mac and switch between them with a single command – no more logging out and back in.

Parsec stores its login in `~/.parsec/user.bin`. `parsec-switch` keeps a copy of that file per profile under `~/.parsec/profiles/<name>/` and swaps it in when you switch. Parsec is quit and relaunched in the process.

## Requirements

- macOS with Parsec installed at `/Applications/Parsec.app`
- zsh (the default shell on macOS)

## Installation

```bash
git clone https://github.com/Ernst-Film/Parsec-Switch.git
mkdir -p ~/.local/bin
ln -s "$PWD/Parsec-Switch/parsec-switch" ~/.local/bin/parsec-switch
```

`~/.local/bin` must be on your `PATH` (e.g. in `~/.zshrc`: `export PATH="$HOME/.local/bin:$PATH"`).

## Setup

Save each login once as a profile:

1. Log in to Parsec with the first account.
2. `parsec-switch save work`
3. `parsec-switch new` – Parsec restarts logged out.
4. Log in to Parsec with the second account.
5. `parsec-switch save personal`

`parsec-switch list` now shows both profiles, the active one marked with `*`.

## Usage

| Command | Effect |
|---|---|
| `parsec-switch` | switch to the next profile (with two profiles: toggle) |
| `parsec-switch <name>` | switch to a specific profile |
| `parsec-switch save <name>` | save the current login as a profile and mark it active |
| `parsec-switch new` | log out for a fresh login, then run `save <name>` |
| `parsec-switch list` | list profiles |
| `parsec-switch delete <name>` | delete a profile |

Before every switch the script copies the current login back into the active profile, so a session that Parsec refreshed in the background is not lost.

## Files

```
~/.parsec/user.bin                  active login (managed by Parsec)
~/.parsec/profiles/<name>/user.bin  saved login per profile
~/.parsec/profiles/.active          name of the active profile
```

> **Warning:** `user.bin` contains your Parsec session. Anyone who has the file is logged in as you. Do not share profiles or put them in cloud folders or repositories.

## Troubleshooting

- **"Profile … does not exist":** check the name with `parsec-switch list`.
- **Parsec shows the login screen after switching:** the profile's session has expired. Log in again in Parsec and overwrite it with `parsec-switch save <name>`.
- **"Toggling needs at least 2 profiles":** create a second profile as described under Setup.

The script only touches Parsec's own daemon (`/Applications/Parsec.app/Contents/MacOS/parsecd`), not macOS's unrelated `parsecd` process from CoreParsec.
