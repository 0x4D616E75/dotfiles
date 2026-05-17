# dotfiles

Dotfiles managed with [chezmoi](https://www.chezmoi.io/).

This setup uses two separate chezmoi source states:

```text
Main Repo
  generic / shared configuration

Secrets Repo
  encrypted / host-specific configuration
```

The separation keeps secrets out of the main repository while still allowing both repositories to behave like full chezmoi source states.

## Prerequisites
```bash
chezmoi
git
age
```

Optional:
```bash
zsh
tmux
nvim
```

## Main Repository
Initialize the generic dotfiles repository:

```bash
chezmoi init https://github.com/0x4D616E75/dotfiles.git
chezmoi apply

git@github.com:0x4D616E75/dotfiles.git
``

This creates the default chezmoi source state at:
```bash
~/.local/share/chezmoi
```

Reload ZSH to enable convience functions (`cm`):
```bash
source ~/.zshrc
```

## Secrets Repository
Clone the secrets repository separately:
```bash
git clone https://github.com/0x4D616E75/example-dotfiles-secrets.git ~/.local/share/chezmoi-secrets
```

Create a separate chezmoi config directory:
```bash
mkdir -p ~/.config/chezmoi-secrets
```

Example config:
```toml
# ~/.config/chezmoi-secrets/chezmoi.toml

encryption = "age"

[age]
identity = "~/.config/chezmoi-secrets/key.txt"
```

Add the age identity:
```bash
mkdir -p ~/.config/chezmoi-secrets
chmod 700 ~/.config/chezmoi-secrets

$EDITOR ~/.config/chezmoi-secrets/key.txt

chmod 600 ~/.config/chezmoi-secrets/key.txt
```

Reload ZSH again to enable convience functions (`cms`):
```bash
source ~/.zshrc
```

Apply the secrets repo:
```bash
cms apply
```

## Usage

Generic dotfiles:
```bash
cm status
cm diff
cm add ~/.zshrc
cm apply
```

Secrets:
```bash
cms status
cms diff
cms add ~/.ssh/config
cms apply
```

## Recommended Separation
Main repo:
```bash
~/.zshrc
~/.vimrc
~/.tmux.conf
~/.config/nvim
```

Secrets repo:
```bash
~/.zshenv
~/.ssh/config
~/.ssh/id_ed25519
tokens
host-specific configuration
```

Avoid managing the same target file in both repositories.
