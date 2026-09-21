# dotfiles

My bash, vim and tmux configuration. It's what I run on my workstation and on
the Linux boxes in my homelab, and most of it leans toward infrastructure work:
Terraform, Ansible, shell and Python.

## Layout

```
bash/bashrc         entry point: PATH, prompt, loads the modules, then ~/.bashrc.local
bash/bash_*         aliases, functions, git/docker/system/security helpers, language setup
vimrc               Vundle plugins, per-language indentation, Syntastic linters
tmux.conf           small single-file tmux config
tmuxprofile/        a few tmux session layouts
gitignore           global gitignore
```

## Install

```sh
git clone https://github.com/defenestratexp/dotfiles.git ~/.dotfiles

ln -sf ~/.dotfiles/bash/bashrc ~/.bashrc
ln -sf ~/.dotfiles/vimrc ~/.vimrc
ln -sf ~/.dotfiles/tmux.conf ~/.tmux.conf
git config --global core.excludesfile ~/.dotfiles/gitignore

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim +PluginInstall +qall
```

`bashrc` finds its modules by following its own symlink. If you keep them
somewhere else, set `DOTFILES_BASH=/path/to/bash` first.

## Per-machine settings

The last thing `bashrc` does is source `~/.bashrc.local` if it exists. That
file isn't in the repo, so it's where account ids, registry names, work aliases
and anything else specific to one machine go:

```bash
# ~/.bashrc.local
export AWS_ACCOUNT_ID=123456789012 AWS_REGION=us-west-2   # used by decrlogin
alias dlab='setdocker deploy@docker-host.example.internal'
```

Because it's loaded last, it can override anything the modules define.

## A few things I use daily

- `conn host` is plain ssh that renames the tmux window to the host first.
- `setdocker user@host` points the docker CLI at a remote engine over SSH;
  `setdocker` on its own switches back to local.
- `decrlogin [region] [account]` logs docker in to ECR, falling back to
  `AWS_REGION` and `AWS_ACCOUNT_ID`.
- In vim, `C-n` toggles NERDTree and Terraform files are formatted on save.
  Python and Terraform indent 4 spaces; JS, HTML, CSS, shell and Ruby indent 2.

## Requirements

- bash 4 or newer. On macOS install a current bash from Homebrew;
  `bashrc` adds `/opt/homebrew/bin` to `PATH` when it's there.
- vim 8+ with Vundle, and a 256-color terminal for the color scheme.
- Optional, used by individual functions: `jq`, `pwgen`, `xclip`/`xsel` or
  `pbcopy`, `openssl`, the `aws` CLI, `docker`, `direnv`, `rbenv`,
  `virtualenvwrapper`, `nvm`, and `tmuxifier` for `tmuxprofile/devops.session.sh`.

## Credits

My bigger tmux setup is built on Alexey Samoshkin's
[tmux-config](https://github.com/samoshkin/tmux-config), which isn't included
here; get it from that repository. The vim color scheme is xoria256 by
Dmitriy Y. Zotikov, installed as a Vundle plugin.

## License

MIT for the files in this repository. See [LICENSE](LICENSE).
