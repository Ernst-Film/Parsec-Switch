# parsec-switch

Mehrere Parsec-Logins auf einem Mac verwalten und per Befehl zwischen ihnen wechseln – ohne jedes Mal aus- und wieder einzuloggen.

Parsec speichert den Login in `~/.parsec/user.bin`. `parsec-switch` hält pro Profil eine Kopie dieser Datei unter `~/.parsec/profiles/<name>/` und tauscht sie beim Wechsel aus. Parsec wird dabei beendet und neu gestartet.

## Voraussetzungen

- macOS mit Parsec unter `/Applications/Parsec.app`
- zsh (Standard-Shell auf macOS)

## Installation

```bash
git clone https://github.com/Ernst-Film/parsec-switch.git
mkdir -p ~/.local/bin
ln -s "$PWD/parsec-switch/parsec-switch" ~/.local/bin/parsec-switch
```

`~/.local/bin` muss im `PATH` stehen (z. B. in `~/.zshrc`: `export PATH="$HOME/.local/bin:$PATH"`).

## Einrichten

Jeden Login einmal als Profil speichern:

1. In Parsec mit dem ersten Konto einloggen.
2. `parsec-switch save arbeit`
3. `parsec-switch new` – Parsec startet ausgeloggt.
4. In Parsec mit dem zweiten Konto einloggen.
5. `parsec-switch save privat`

`parsec-switch list` zeigt danach beide Profile, das aktive mit `*`.

## Benutzung

| Befehl | Wirkung |
|---|---|
| `parsec-switch` | zum nächsten Profil wechseln (bei zwei Profilen: hin und her) |
| `parsec-switch <name>` | zu einem bestimmten Profil wechseln |
| `parsec-switch save <name>` | aktuellen Login als Profil speichern und aktiv setzen |
| `parsec-switch new` | ausloggen für einen frischen Login, danach `save <name>` |
| `parsec-switch list` | Profile anzeigen |
| `parsec-switch delete <name>` | Profil löschen |

Vor jedem Wechsel sichert das Skript den aktuellen Login zurück ins aktive Profil. Erneuert Parsec die Sitzung im Hintergrund, bleibt das Profil damit aktuell.

## Dateien

```
~/.parsec/user.bin                 aktiver Login (von Parsec verwaltet)
~/.parsec/profiles/<name>/user.bin gespeicherter Login je Profil
~/.parsec/profiles/.active         Name des aktiven Profils
```

> **Achtung:** `user.bin` enthält die Parsec-Sitzung. Wer die Datei hat, ist mit diesem Konto eingeloggt. Profile nicht weitergeben, nicht in Cloud-Ordner oder Repos legen.

## Wenn es nicht klappt

- **„Profil existiert nicht“:** Name mit `parsec-switch list` prüfen.
- **Parsec zeigt nach dem Wechsel den Login-Bildschirm:** Die Sitzung des Profils ist abgelaufen. In Parsec neu einloggen und mit `parsec-switch save <name>` überschreiben.
- **„Zum Togglen braucht es mindestens 2 Profile“:** Zweites Profil wie unter „Einrichten“ anlegen.
