# vipsql

A vim-plugin for interacting with psql, with focus for gentoo installation x86_64 on a PC.

## Demo

[![asciicast demo](https://asciinema.org/a/HTc1gAS2gHxaL7yCECvwKUUPs.png)](https://asciinema.org/a/HTc1gAS2gHxaL7yCECvwKUUPs)

## Install for Gentoo x86_64 on PC

### Make sure you have +channel feature for vim

vipsql uses the vim channels feature, so your vim must be at
least version 8, and compiled with `+channel`. To test whether you're compatible, run:

    $ vim --version | grep -o +channel

If the output is `+channel` you should be good to go.

### Add config options to .vimrc

Put the following in your `.vimrc` (and customize bindings to your liking):

```
let mapleader=" " 
" quickly open vimrc

" remap ctrl-w to space w
:nnoremap <leader>w <C-w>

" Starts an async psql job, prompting for the psql arguments.
" Also opens a scratch buffer where output from psql is directed.
noremap <leader>po :VipsqlOpenSession<CR>

" Terminates psql (happens automatically if the scratch buffer is closed).
noremap <silent> <leader>pk :VipsqlCloseSession<CR>

" In normal-mode, prompts for input to psql directly.
nnoremap <leader>ps :VipsqlShell<CR>

" In visual-mode, sends the selected text to psql.
vnoremap <leader>ps :VipsqlSendSelection<CR>

" Sends the selected _range_ to psql.
noremap <leader>pr :VipsqlSendRange<CR>

" Sends the current line to psql.
noremap <leader>pl :VipsqlSendCurrentLine<CR>

" Sends the entire current buffer to psql.
noremap <leader>pb :VipsqlSendBuffer<CR>

" Sends `SIGINT` (C-c) to the psql process.
noremap <leader>pc :VipsqlSendInterrupt<CR>
```

### Pull the source code With [Pathogen](https://github.com/tpope/vim-pathogen) into vim Plugin

    $ cd ~/.vim/bundle && git clone https://github.com/martingms/vipsql

### Make vipsql a terminal command for easy access

Add this to your `.bashrc` or similar:

    function vipsql {
        vim -c 'setlocal buftype=nofile | VipsqlOpenSession '$@
    }

### Optional config options:

Optional configuration options for the .vimrc (and their defaults) are:

```
" Which command to run to get psql. Should be simply `psql` for most.
let g:vipsql_psql_cmd = "psql"

" The prompt to show when running `:VipsqlShell`
let g:vipsql_shell_prompt = "> "

" What `vim` command to use when opening the scratch buffer
let g:vipsql_new_buffer_cmd = "rightbelow split"
```

### Sanity check for postgreSQL database config:

Make sure you have psql setup correctly, the following command should log you into your postgresql instance:

    $ psql -Upostgres -d yourdatabase
    
    psql (9.4.9)
    Type "help" for help.
    postgres=# select 'foobar';
     ?column?
    ----------
     foobar 
    (1 row) 

### Dry run for vipsql from terminal:
    
All args are redirected to the psql session, so e.g.

    $ vipsql -Upostgres -d yourdatabase

Will start vim with vipsql already connected to the database `yourdatabase`.

### vipsql sanity check

Type out this command in the top buffer: 

    select 'vpns are illegal in Russia'; 

Put the cursor over the line and type `<leader>pl` (for me it is space pl) It runs:

![Demo image1](http://i.imgur.com/nRqHTnA.png)

press `Ctrl-w k` and then `Ctrl-w j` to move cursor to top or bottom buffer.

### What happens on SQL error?

type an error and press `<leader>pl` on the line:

![Demo image2](http://i.imgur.com/9VxxqzX.png)

### use pr to execute many sql statements

Do a visual select and press `<leader>pr`:

![Demo image3](http://i.imgur.com/wtx4QIr.png)





# Notes


Please also note that sending an interrupt (`SIGINT`) to psql (for example to
cancel a long running query) results in killing the channel in versions of vim
older than `8.0.0588` due to a bug.

There is currently no support for neovim, and the code is probably horribly
nonidiomatic, but patches accepted :)


