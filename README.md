# **Deprecated**

This project is deprecated.

Please use [zeno.zsh](https://github.com/yuki-yano/zeno.zsh).

# fzf-preview.zsh

fzf-completion plugin

**WARNING: This project is in beta stage. Any changes are applied without announcements.**

## Demo

![fzf-preview.zsh](https://user-images.githubusercontent.com/5423775/80238632-b8fe1900-8699-11ea-9d4f-0856cc0df95d.gif "fzf-preview.zsh")

## Usage

### Completion

The completion for the configured command is switched to completion using fzf.

Example:

```shell
$ git add <Tab>
Git Add Files> ...
```

### Command and Widget

Some useful fzf-based ZLE widgets are provided.

- fzf-cd
- fzf-ghq
- fzf-grep-vscode
- fzf-history-selection

Default keybind is

```shell
bindkey '^r'   fzf-history-selection
bindkey '^x^c' fzf-cd
bindkey '^x^g' fzf-ghq
bindkey '^x^v' fzf-grep-vscode
```

To use fzf-tmux in tmux session, set `FZF_PREVIEW_ENABLE_TMUX` to 1. (Default: 0)
To disable this setting, set `FZF_PREVIEW_DISABLE_DEFAULT_BIND` to 1.

### Snippet

WIP

## Requirements

- [junegunn/fzf: A command-line fuzzy finder](https://github.com/junegunn/fzf)

### Optional

- [sharkdp/bat: A cat(1) clone with wings.](https://github.com/sharkdp/bat)
- [ogham/exa: A modern version of ‘ls’.](https://github.com/ogham/exa)
- [sharkdp/fd: A simple, fast and user-friendly alternative to 'find'](https://github.com/sharkdp/fd)
- [BurntSushi/ripgrep: ripgrep recursively searches directories for a regex pattern](https://github.com/BurntSushi/ripgrep/)
- [x-motemen/ghq: Remote repository management made easy](https://github.com/x-motemen/ghq)
- [Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

## Installation

### zinit

```zsh
zinit ice lucid depth"1" blockf
zinit light yuki-ycino/fzf-preview.zsh
```

or

```shell
$ git clone https://github.com/yuki-ycino/fzf-preview.zsh.git
$ echo "source /path/to/dir/fzf-preview.zsh" >> ~/.zshrc
```

### Add .zshrc

```zsh
bindkey '^i' fzf-or-normal-completion
```

## Completion

- git
  - add
  - commit --fixup
  - branch (-d|-D|-m|-M) (--merged|--remotes)
  - log
  - diff (--color-words)
  - diff (--color-words) --
  - checkout [commit]
  - checkout [commit] --
  - checkout (-t|--track)
  - restore
  - restore --source
  - restore --source [commit]
  - switch
  - rebase (-i|--interactive) (--autostash)
  - merge
  - revert
  - stash (show|pop|apply|drop)
  - cherry-pick

- gh
  - issue view (--web)
  - pr view (--web)
  - checkout

## User Configuration

```shell
FZF_PREVIEW_DISABLE_DEFAULT_SETTING=1 # If you want to disable the default settings
```

The default configuration directory is `${XDG_CONFIG_HOME}/fzf-preview.zsh`.
It is also possible to set `FZF_PREVIEW_USER_CONFIG_DIR`.

Refer to the `fzf-preview.zsh/config` directory and add the `symbol-{foo}` and `command-{foo_bar}` files to the create a new directory in the `XDG_CONFIG_HOME/fzf-preview.zsh/completion` directory.

The symbol and command are simple zsh scripts.
Define the command in symbol using an associative array and put the fzf setting in the command file.

gh example

### symbol-foo

```zsh:symbol-foo
#autoload

symbols+=(
  '(^foo bar $)' foo_bar
)
```

### command-foo_bar

```zsh:command-foo_bar
#autoload

typeset id="foo_bar"
typeset candidate_command="foo bar"
typeset -a fzf_options=(
  "${FZF_PREVIEW_DEFAULT_SETTING}"
)
typeset -a bind=(
  "${FZF_PREVIEW_DEFAULT_BIND}"
)
fzf_options+=("--bind=\"${(j:,:)bind}\"")
local fzf_command="fzf ${(j: :)fzf_options}"
typeset callback=("awk '{ print \$1 }'")

fzf-preview-complete $candidate_command $fzf_command $callback
```

## Related Project

[fzf-zsh-completions](https://github.com/chitoku-k/fzf-zsh-completions)
