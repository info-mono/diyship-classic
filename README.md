<p align="center"><a href="https://www.figma.com/file/ivfF5xzAi1zioAkpDHbUyb/DIYship"><img src="https://user-images.githubusercontent.com/43980777/142657083-ec10c6a1-b34b-4517-9110-3d07f7263a63.png"></a></p>
<p align="center">Cross-shell prompt framework</p>
<p align="center"><a href="https://github.com/info-mono/diyship-classic/blob/main/LICENSE"><img src="https://img.shields.io/github/license/info-mono/diyship-classic?labelColor=383838&color=585858&style=for-the-badge" alt="License: GPL-3.0"></a> <a href="https://gist.github.com/NNBnh/9ef453aba3efce26046e0d3119dab5a7#development-completed"><img src="https://img.shields.io/badge/development-completed-%23585858.svg?labelColor=383838&style=for-the-badge&logoColor=FFFFFF" alt="Development completed"></a></p>

## ๐ก About

**DIYship** is a cross-shell prompt framework written in [`portable sh`](https://github.com/dylanaraps/pure-sh-bible) that let you write your prompt with any programing language for any shell.

> _[Learn more about how its work](#%EF%B8%8F-configuration)._

## ๐ Setup

### ๐งพ Dependencies

- [`sh`](https://wikipedia.org/wiki/Bourne_shell) to process.
- [`date`](https://wikipedia.org/wiki/Unix_time) and [`cut`](https://wikipedia.org/wiki/Cut_(Unix)) for timing (required by Bash, Zsh, Elvish and Tcsh).
- [`wc`](https://wikipedia.org/wiki/Wc_(Unix)) for job counting (required by Ion).
- [`cat`](https://wikipedia.org/wiki/Cat_(Unix)) for return workaround (required by Xonsh).

### ๐ฅ Installation

#### ๐ง Manually

Option 1: using `curl`

```sh
curl https://raw.githubusercontent.com/info-mono/diyship-classic/main/bin/diyship > ~/.local/bin/diyship
chmod +x ~/.local/bin/diyship
```

Option 2: using `git`

```sh
git clone https://github.com/info-mono/diyship-classic.git ~/.local/share/diyship
ln -s ~/.local/share/diyship/bin/diyship ~/.local/bin/diyship
```

#### ๐ฆ Package manager

For [Bpkg](https://github.com/bpkg/bpkg) user:

```sh
bpkg install info-mono/diyship-classic
```

For [Basher](https://github.com/basherpm/basher) user:

```sh
basher install info-mono/diyship-classic
```

> _If you can and want to port DIYship to other package managers, feel free to do so._

## โจ๏ธ Usage

Add the init script to your shell's config file:

### ๐ Bash

Add the following to the end of `~/.bashrc`:

```bash
eval "$(diyship bash)"
```

### ๐ Zsh

Add the following to the end of `~/.zshrc`:

```zsh
eval "$(diyship zsh)"
```

### ๐ Fish

Add the following to the end of `~/.config/fish/config.fish`:

```fish
diyship fish | source
```

### ๐ PowerShell

Add the following to the end of `Microsoft.PowerShell_profile.ps1`.
You can check the location of this file by querying the `$PROFILE` variable in PowerShell.
Typically the path is `~\Documents\PowerShell\Microsoft.PowerShell_profile.ps1` or `~/.config/powershell/Microsoft.PowerShell_profile.ps1` on `*nix`.

```powershell
Invoke-Expression (@(&diyship powershell) -join "`n")
```

### ๐ Ion

Add the following to the end of `~/.config/ion/initrc`:

```ion
eval $(diyship ion)
```

### ๐ Elvish

Add the following to the end of `~/.elvish/rc.elv`:

```elv
eval (diyship elvish | slurp)
```

> _Only Elvish v0.15 or higher is supported._

### ๐ Tcsh

Add the following to the end of `~/.tcshrc`:

```tcsh
eval `diyship tcsh`
```

### ๐ Nushell

Add the following to your Nushell config file. You can check the location of this file by running `config path` in Nushell.

```toml
startup = [
	"mkdir ~/.cache/diyship",
	"diyship nushell | save ~/.cache/diyship/init.nu",
	"source ~/.cache/diyship/init.nu"
]

prompt = "diyship_prompt"
```

### ๐ Xonsh

Add the following to the end of `~/.xonshrc`:

```xsh
execx($(diyship xonsh))
```

> _Only Nushell version v0.33 or higher is supported._

## โ๏ธ Configuration

DIYship is basically execute a command (which could be a path to an **executable** script file or program) and take it output as a prompt,
you can change the command through environment variable:<br>
`export DIYSHIP_COMMAND_<POSITION>="<command>"`

| Environment variable    | Default                          | Description                    |
| ----------------------- | -------------------------------- | ------------------------------ |
| `DIYSHIP_COMMAND_LEFT`  | `$XDG_CONFIG_HOME/diyship/left`  | Command to print left prompt.  |
| `DIYSHIP_COMMAND_RIGHT` | `$XDG_CONFIG_HOME/diyship/right` | Command to print right prompt. |

DIYship will export these following environment variables before running the commands to print out prompts,
so you could utilize theme informations in your script/program:

| Environment variable  | Description                                                   |
| --------------------- | ------------------------------------------------------------- |
| `$DIYSHIP_SHELL`      | Current shell name.                                           |
| `$DIYSHIP_STATUS`     | The status code of the previously run command.                |
| `$DIYSHIP_PIPESTATUS` | Status codes from a command pipeline.                         |
| `$DIYSHIP_DURATION`   | The execution duration of the last command (in milliseconds). |
| `$DIYSHIP_JOBS`       | The number of currently running jobs                          |
| `$DIYSHIP_KEYMAP`     | The current keymap.                                           |

Do to many technical limitation, not every shell support all features:

| Feature              | Bash | Zsh | Fish | Powershell | Ion | Elvish | Tcsh | Nushell | Xonsh |
| -------------------- | ---- | --- | ---- | ---------- | --- | ------ | ---- | ------- | ----- |
| Left prompt          | โ   | โ  | โ   | โ         | โ  | โ     | โ   | โ      | โ    |
| Right prompt         |      | โ  | โ   |            |     | โ     |      |         | โ    |
| Export current shell | โ   | โ  | โ   | โ         | โ  | โ     | โ   | โ      | โ    |
| Export status        | โ   | โ  | โ   | โ         | โ  |        | โ   |         | โ    |
| Export pipe status   | โ   | โ  | โ   |            |     |        |      |         |       |
| Export duration      | โ   | โ  | โ   | โ         | โ  | โ     | โ   | โ      | โ    |
| Export jobs          | โ   | โ  | โ   | โ         | โ  | โ     |      |         | โ    |
| Export keymap        |      | โ  | โ   |            |     |        |      |         |       |

> _For more documentation and inspiration about writing your own prompt, checkout [ours wiki](https://github.com/info-mono/diyship/wiki) and [Starship's custom modules](https://github.com/starship/starship/discussions/1252)._

## ๐ Credits

This project was heavily based on and inspired by [**Starship**](https://starship.rs).

<br><br><br><br>

---

> <h1 align="center">Made with โค๏ธ by <a href="https://github.com/info-mono"><code>@info-mono</code></a></h1>
>
> <p align="center"><a href="https://www.buymeacoffee.com/nnbnh"><img src="https://img.shields.io/badge/buy_me_a_coffee%20-%23F7CA88.svg?logo=buy-me-a-coffee&logoColor=333333&style=for-the-badge" alt="Buy Me a Coffee"></a></p>
