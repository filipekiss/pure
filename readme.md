# Pure

This is a fork from [sindresorhus/pure]. My repository is synced with the
upstream every time I make a change (or, at least, once a month). The contents
of the file below may have been modified to reflect the changes I've made. Check
out the [original] if you like.

### Notable changes

So far, the changes I've made are basically eye candy. (But, hey, I spend the
day looking at my prompt. Might as well enjoy it)

-   Added instructions to install using [zplug] and removed npm ones (since this
    fork, specifically, can't be installed via npm)
-   Added custom symbol for prompt errors instead of only changing the color to
    red

> Pretty, minimal and fast ZSH prompt

![screenshot]

## Overview

Most prompts are cluttered, ugly and slow. I wanted something visually pleasing
that stayed out of my way.

### Why?

-   Shows `git` branch and whether it's dirty (with a `*`).
-   Indicates when you have unpushed/unpulled `git` commits with up/down arrows.
    _(Check is done asynchronously!)_
-   Prompt character changes and turns red if the last command didn't exit with
    `0`.
-   Command execution time will be displayed if it exceeds the set threshold.
-   Username and host only displayed when in an SSH session.
-   Shows the current path in the title and the
    [current folder & command](screenshot-title-cmd.png) when a process is
    running.
-   Support VI-mode indication by reverse prompt symbol (Zsh 5.3+).
-   Makes an excellent starting point for your own custom prompt.

## Install

Can be installed with [zplug] or manually. Requires Git 2.0.0+ and ZSH 5.2+.
Older versions of ZSH are known to work, but they are **not** recommended.

### zplug

If you're using [zplug] to manage your zsh plugins, installing it is as easy as
adding the line below to your `.zshrc`

```
zplug "filipekiss/pure", depth:1, use:"{async,pure}.zsh"
```

If you're not using zplug yet, here's a sample config to get you started (just
add this to the beginning of your `.zshrc` file):

```
#----------------------------------------
# zPlug
#----------------------------------------

if [[ ! -f ~/.zplug/init.zsh ]]; then
  if (( $+commands[git] )); then
    git clone https://github.com/zplug/zplug ~/.zplug
  else
    echo 'git not found' >&2
    exit 1
  fi
fi

source ~/.zplug/init.zsh

#----------------------------------------
# zPlug modules
#----------------------------------------

zplug "zplug/zplug", hook-build:"zplug --self-manage"
zplug "filipekiss/pure", depth:1, use:"{async,pure}.zsh"

#----------------------------------------
# Install missing modules
#----------------------------------------

if ! zplug check --verbose; then
  printf "Install? [y/N]: "
  if read -q; then
    echo; zplug install
  fi
fi

zplug load

```

That's it. Skip to [Getting started](#getting-started).

### Manually

1. Either…

-   Clone this repo
-   add it as a submodule, or
-   just download [`pure.zsh`](pure.zsh) and [`async.zsh`](async.zsh)

2. Symlink `pure.zsh` to somewhere in
   [`$fpath`](https://www.refining-linux.org/archives/46-ZSH-Gem-12-Autoloading-functions.html)
   with the name `prompt_pure_setup`.

3. Symlink `async.zsh` in `$fpath` with the name `async`.

#### Example

```console
$ ln -s "$PWD/pure.zsh" /usr/local/share/zsh/site-functions/prompt_pure_setup
$ ln -s "$PWD/async.zsh" /usr/local/share/zsh/site-functions/async
```

_Run `echo $fpath` to see possible locations._

For a user-specific installation (which would not require escalated privileges),
simply add a directory to `$fpath` for that user:

```sh
# .zshenv or .zshrc
fpath=( "$HOME/.zfunctions" $fpath )
```

Then install the theme there:

```console
$ ln -s "$PWD/pure.zsh" "$HOME/.zfunctions/prompt_pure_setup"
$ ln -s "$PWD/async.zsh" "$HOME/.zfunctions/async"
```

## Getting started

Initialize the prompt system (if not so already) and choose `pure`:

```sh
# .zshrc
autoload -U promptinit; promptinit
prompt pure
```

## Options

| Option                           | Description                                                                                    | Default value  |
| :------------------------------- | :--------------------------------------------------------------------------------------------- | :------------- |
| **`PURE_CMD_MAX_EXEC_TIME`**     | The max execution time of a process before its run time is shown when it exits.                | `5` seconds    |
| **`PURE_GIT_PULL=0`**            | Prevents Pure from checking whether the current Git remote has been updated.                   |                |
| **`PURE_GIT_UNTRACKED_DIRTY=0`** | Do not include untracked files in dirtiness check. Mostly useful on large repos (like WebKit). |                |
| **`PURE_GIT_DELAY_DIRTY_CHECK`** | Time in seconds to delay git dirty checking when `git status` takes > 5 seconds.               | `1800` seconds |
| **`PURE_PROMPT_SYMBOL`**         | Defines the prompt symbol.                                                                     | `❯`            |
| **`PURE_PROMPT_VICMD_SYMBOL`**   | Defines the prompt symbol used when the `vicmd` keymap is active (VI-mode).                    | `❮`            |
| **`PURE_GIT_DOWN_ARROW`**        | Defines the git down arrow symbol.                                                             | `⇣`            |
| **`PURE_GIT_UP_ARROW`**          | Defines the git up arrow symbol.                                                               | `⇡`            |

## Example

```sh
# .zshrc

autoload -U promptinit; promptinit

# optionally define some options
PURE_CMD_MAX_EXEC_TIME=10

prompt pure
```

## Tips

In the screenshot you see Pure running in [Hyper](https://hyper.is) with the
[hyper-snazzy](https://github.com/sindresorhus/hyper-snazzy) theme and Menlo
font.

The [Tomorrow Night Eighties](https://github.com/chriskempson/tomorrow-theme)
theme with the
[Droid Sans Mono](https://www.fontsquirrel.com/fonts/droid-sans-mono) font
(15pt) is also a
[nice combination](https://github.com/sindresorhus/pure/blob/95ee3e7618c6e2162a1e3cdac2a88a20ac3beb27/screenshot.png).<br>
_Just make sure you have anti-aliasing enabled in your terminal._

To have commands colorized as seen in the screenshot, install
[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting).

## Integration

### [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

1. Set `ZSH_THEME=""` in your `.zshrc` to disable oh-my-zsh themes.
2. Follow the Pure [Install](#install) instructions.
3. Do not enable the following (incompatible) plugins: `vi-mode`, `virtualenv`.

**NOTE:** `oh-my-zsh` overrides the prompt so Pure must be activated _after_
`source $ZSH/oh-my-zsh.sh`.

### [prezto](https://github.com/sorin-ionescu/prezto)

Pure is bundled with Prezto. No need to install it.

Add `prompt pure` to your `~/.zpreztorc`.

### [zim](https://github.com/Eriner/zim)

Pure is bundled with Zim. No need to install it.

Set `zprompt_theme='pure'` in `~/.zimrc`.

### [antigen](https://github.com/zsh-users/antigen)

Update your `.zshrc` file with the following two lines (order matters). Do not
use the `antigen theme` function.

```sh
antigen bundle mafredri/zsh-async
antigen bundle sindresorhus/pure
```

### [antibody](https://github.com/getantibody/antibody)

Update your `.zshrc` file with the following two lines (order matters):

```sh
antibody bundle mafredri/zsh-async
antibody bundle sindresorhus/pure
```

### [zplug]

Update your `.zshrc` file with the following two lines:

```sh
zplug mafredri/zsh-async, from:github
zplug filipekiss/pure, use:pure.zsh, from:github, as:theme
```

### [zplugin](https://github.com/zdharma/zplugin)

Update your `.zshrc` file with the following two lines (order matters):

```sh
zplugin ice pick"async.zsh" src"pure.zsh"
zplugin light sindresorhus/pure
```

## FAQ

See [FAQ Archive](https://github.com/sindresorhus/pure/wiki/FAQ-Archive) for
previous FAQs.

## Ports

-   **ZSH** - [therealklanni/purity](https://github.com/therealklanni/purity) -
    More compact current working directory, important details on the main prompt
    line, and extra Git indicators. -
    [intelfx/pure](https://github.com/intelfx/pure) - Solarized-friendly colors,
    highly verbose, and fully async Git integration. -
    [dfurnes/purer](https://github.com/dfurnes/purer) - Compact single-line
    prompt with built-in Vim-mode indicator. -
    [chabou/pure-now](https://github.com/chabou/pure-now) - Fork with
    [Now](https://zeit.co/now) support
-   **Bash** - [sapegin/dotfiles](https://github.com/sapegin/dotfiles) -
    [Prompt](https://github.com/sapegin/dotfiles/blob/dd063f9c30de7d2234e8accdb5272a5cc0a3388b/includes/bash_prompt.bash)
    and [color theme](https://github.com/sapegin/dotfiles/tree/master/color) for
    Terminal.app.
-   **Fish** -
    [brandonweiss/pure.fish](https://github.com/brandonweiss/pure.fish) -
    Pure-inspired prompt for Fish. Not intended to have feature parity. -
    [rafaelrinaldi/pure](https://github.com/rafaelrinaldi/pure) - Support for
    bare Fish and various framework
    ([Oh-My-Fish](https://github.com//oh-my-fish/oh-my-fish),
    [Fisherman](https://github.com//fisherman/fisherman), and
    [Wahoo](https://github.com//bucaran/wahoo)).
-   **Rust** - [xcambar/purs](https://github.com/xcambar/purs) - Pure-inspired
    prompt in Rust.
-   **Go** - [talal/mimir](https://github.com/talal/mimir) - Pure-inspired
    prompt in Go with Kubernetes and OpenStack cloud support. Not intended to
    have feature parity.
-   **PowerShell** -
    [nickcox/pure-pwsh](https://github.com/nickcox/pure-pwsh/) - PowerShell/PS
    Core implementation of the Pure prompt.

## Team

| [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](http://sindresorhus.com) | [![Mathias Fredriksson](https://github.com/mafredri.png?size=100)](https://github.com/mafredri) | [![Filipe Kiss](https://github.com/filipekiss.png?size=100)](http://filipekiss.com.br) |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| [Sindre Sorhus](https://github.com/sindresorhus)                                          | [Mathias Fredriksson](https://github.com/mafredri)                                              | [Filipe Kiss](https://github.com/filipekiss)                                           |

## License

Original Work - MIT © [Sindre Sorhus](https://sindresorhus.com)

Further Modifications - MIT © [Filipe Kiss]

[filipe kiss]: http://github.com/filipekiss
[screenshot]: screenshot.png
[sindresorhus/pure]: https://github.com/sindresorhus/pure
[original]: https://github.com/sindresorhus/pure/blob/master/readme.md
[zplug]: https://github.com/zplug/zplug
