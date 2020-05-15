# vimrc

Organize my vim life.

The aim is to have a "nice" vim experience that is portable from desktop/laptop to servers.

```
cd ~
mv .vim .vim_back
mv .vimrc .vimrc_back
git clone https://github.com/dganellari/vimrc.git .vim
ln -s .vim/vimrc .vimrc
# neovim support
mkdir -p .config/nvim && ln -s ~/.vim/init.vim .config/nvim/init.vim
# to install all plugins
vim +PlugInstal +qall # skip +qall if you want to see the results
cd ~/.vim/plugged/YouCompleteMe
./install.py --clangd-completer --rust-completer
```

For neovim you need the neovim python module installed, otherwise YouCompleteMe does not work.
Either install it with your package manager or create a virtual environment
```
python3 -m venv $HOME/python_venv
$HOME/python_venv/bin/activate
pip install neovim
```
Also make sure, that you add `$HOME/python_venv/bin/activate` to your .bashrc (or equivalent)

For more details regarding fzf installation please have a look at the following link: https://github.com/junegunn/fzf/blob/master/README-VIM.md.


For vim within tmux you will need the following plugin: https://github.com/christoomey/vim-tmux-navigator.

However, when using fzf within tmux, ctrl-k and ctrl-j for moving up and down its search list will not work if you are using the vim-tmux navigator's suggested bindings in your .tmux.conf. For that to work one need to add the following bindings instead so that tmux will treat fzf as it treats vim:

```
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \ 
| grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"

is_fzf="ps -o state= -o comm= -t '#{pane_tty}' \
  | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?fzf$'"
  
bind -n C-h run "($is_vim && tmux send-keys C-h) || \
                          tmux select-pane -L"
                          
bind -n C-j run "($is_vim && tmux send-keys C-j)  || \
                         ($is_fzf && tmux send-keys C-j) || \
                         tmux select-pane -D"
                         
bind -n C-k run "($is_vim && tmux send-keys C-k) || \
                          ($is_fzf && tmux send-keys C-k)  || \
                          tmux select-pane -U"
                          
bind -n C-l run  "($is_vim && tmux send-keys C-l) || \
                          tmux select-pane -R"
                          
bind-key -n C-\ if-shell "$is_vim" "send-keys C-\\" "select-pane -l"
```
