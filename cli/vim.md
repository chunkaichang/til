# VIM

## Normal mode shortcuts

- `w`: jumps to next word, `b`: beginning of word, `e`: end of word
- `ci(`: change the contents inside current `()` 
- `ci'`: change the contents of current string (similarly for `"`)

## Useful config

- `set number` and `set relativenumber` helps jump to certain line reltively with `{cnt}j` and `{cnt}k`.

- For ease of copy-paste using mouse, I add a mapping to toggle line numbers.

```
let mapleader = ","

" show hybrid line numbers
set number
set relativenumber
" map for toggling line numbers
nmap <silent> <leader>l :set nu! rnu!<CR>

set incsearch
set mouse+=a

colorscheme elflord

filetype plugin indent on
" show existing tab with 4 spaces width
set tabstop=4
" when indenting with '>', use 4 spaces width
set shiftwidth=4
" On pressing tab, insert 4 spaces
set expandtab

" spell checking
set spelllang=en_us
set spellfile=$HOME/.vim/en.utf-8.add
nmap <silent> <leader>s :set spell!<CR>
hi clear SpellBad
hi SpellBad cterm=underline

" ctags
set tags=tags;/
```
