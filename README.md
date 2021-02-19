# vim-as-a-presentation-tool

Gathered Information and Files to turning VIM to a presentation Tool

## Table of Content

<a name="toc"></a>

1. [Sources](#Sources)
1. [Nick's approach](#NicksApproach)
   1. [config:](#nick_config:)
1. [Sebastian's approach](#SebastiansApproach)
   1. [VIM with specific Settings](#specificSettings)
   1. [Snippet to include special Characters](#Snippet)
   1. [ASCII ART and Border](#ASCII-ART-and-Border)
      1. [config](#config)
      1. [Addons](#Addons)
         1. [cowsay](#cowsay)
         1. [fortune](#fortune)
         1. [linuxlogo](#linuxlogo)
         1. [boxes](#boxes)
         1. [lolcat](#lolcat)
         1. [Examples](#Examples)
   1. [Pictures](#Pictures)
   1. [Highlighting](#Highlighting)

<a name="Sources"></a>

## Sources [↸](#toc)

- [Giving a Text Based Slide Presentation in Vim without Plugins (Nick Janetakis, 26.09.2020)](https://youtu.be/7fIR55kkTwc)
- [Creating technical presentations with VIM (Sebastian Daschner, 21.07.2020)](https://youtu.be/GDa7hrbcCB8)
  - his
    - corresponding [Blog Article](https://blog.sebastian-daschner.com/entries/presentations-with-vim)
    - [.vimrc](https://github.com/sdaschner/dotfiles/blob/master/.vimrc)
    - [snippets](https://github.com/sdaschner/dotfiles/blob/master/.vim/UltiSnips/all.snippets)

<a name="NicksApproach"></a>

## Nick's approach [↸](#toc)

- slides are named with `vpm` extension for Vim Presentation Mode
- change to slide directory and open all files in the buffer
  - `nvim *`
  - list all opened files: `:ls`
- optional Plugin: [junegunn/goyo.vim](https://github.com/junegunn/goyo.vim)
  - Distraction-free writing in Vim

<a name="nich_config:"></a>

### config: [↸](#toc)

```vim
  " Mappings to make Vim more freindly towards presenting slides.
  autocmd BufNewFile,BufRead *.vpm call SetVimPresentationMode()
  function SetVimPresentationMode()
    nnoremap <buffer> <Right> :n<CR>
    nnoremap <buffer> <Right> :n<CR>

    if !exists('#goyo')
      Goyo
    endif
  endfunction
```
<a name="SebastiansApproach"></a>

## Sebastian's approach [↸](#toc)

<a name="specificSettings"></a>

### VIM with specific Settings [↸](#toc)

F5 to switch to Presentation Mode

```vim
  " presentation mode
    noremap <Left> :silent bp<CR> :redraw!<CR>
    noremap <Right> :silent bn<CR> :redraw!<CR>
    nmap <F5> :set relativenumber! number! showmode! showcmd! hidden! ruler!<CR>
    nmap <F2> :call DisplayPresentationBoundaries()<CR>
    nmap <F3> :call FindExecuteCommand()<CR>

  " jump to slides
    nmap <F9> :call JumpFirstBuffer()<CR> :redraw!<CR>
    nmap <F10> :call JumpSecondToLastBuffer()<CR> :redraw!<CR>
    nmap <F11> :call JumpLastBuffer()<CR> :redraw!<CR>



  let g:presentationBoundsDisplayed = 0
  function! DisplayPresentationBoundaries()
    if g:presentationBoundsDisplayed
      match
      set colorcolumn=0
      let g:presentationBoundsDisplayed = 0
    else
      highlight lastoflines ctermbg=darkred guibg=darkred
      match lastoflines /\%23l/
      set colorcolumn=80
      let g:presentationBoundsDisplayed = 1
    endif
  endfunction
```

```vim
  " Automatically source an eponymous <file>.vim or <file>.<ext>.vim if it exists, as a bulked-up
  " modeline and to provide file-specific customizations.
  function! s:AutoSource()
      let l:testedScripts = ['syntax.vim']
      if expand('<afile>:e') !=# 'vim'
          " Don't source the edited Vimscript file itself.
          call add(l:testedScripts, 'syntax.vim')
      endif

      for l:filespec in l:testedScripts
          if filereadable(l:filespec)
              execute 'source' fnameescape(l:filespec)
          endif
      endfor

      call FindExecuteCommand()
  endfunction
  augroup AutoSource
      autocmd! BufNewFile,BufRead * call <SID>AutoSource()
  augroup END
```

<a name="Snippet"></a>

### Snippet to include special Characters [↸](#toc)

<a name="ASCII-ART-and-Border"></a>

### ASCII ART and Border [↸](#toc)

- command line tool: [toilet](http://manpages.ubuntu.com/manpages/bionic/man1/toilet.1.html)
  - `sudo apt-get update`
  - `sudo apt-get install toilet`
  - **Examples**
    - `toilet -f smblock --filter border 'Linux is fun!'`
    - [some examples](https://delightlylinux.wordpress.com/2015/11/13/colored-text-with-toilet/)
  - use same fonts like [figlet](http://manpages.ubuntu.com/manpages/precise/man6/figlet.6.html): `/usr/share/figlet`
  - [more Fonts](http://www.figlet.org/)
  - [my figlet font selection](https://github.com/dele1972/my-figlet-font-selection#my-figlet-font-selection)

<a name="config"></a>

#### config [↸](#toc)

```vim
  " adds a line of <
    nmap <leader>a :normal 20i<<CR>

  " makes Ascii art font
    nmap <leader>F :.!toilet -w 200 -f standard<CR>
    nmap <leader>f :.!toilet -w 200 -f small<CR>
  " makes Ascii border
    nmap <leader>1 :.!toilet -w 200 -f term -F border<CR>
```

<a name="Addons"></a>

#### Addons [↸](#toc)

<a name="cowsay"></a>

##### cowsay [↸](#toc)

    - [cowsay](https://en.wikipedia.org/wiki/Cowsay)
      - `sudo apt-get install cowsay`

<a name="fortune"></a>

##### fortune [↸](#toc)

    - fortune
      - `sudo apt-get install fortune`

<a name="linuxlogo"></a>

##### linuxlogo [↸](#toc)

    - linuxlogo
      - `sudo apt-get install linuxlogo`
      - `linuxlogo -a -l`

<a name="boxes"></a>

##### boxes [↸](#toc)

    - boxes
      - `sudo apt-get install boxes`

<a name="lolcat"></a>

##### lolcat [↸](#toc)

    - lolcat
      - `sudo apt-get install boxes`

<a name="Examples"></a>

##### Examples [↸](#toc)

      - `fortune | cowsay -f eyes | toilet --metal -f term`
      - `linuxlogo -a -l | toilet --metal -f term`
      - `toilet -f future 'Linux is fun!' | boxes -d cat -a hc -p h8 | lolcat`

<a name="Pictures"></a>

### Pictures [↸](#toc)

- scripting
- out of slide: `!!:open cover.png`

```vim
  function! FindExecuteCommand()
    let line = search('\S*!'.'!:.*')
    if line > 0
      let command = substitute(getline(line), "\S*!"."!:*", "", "")
      execute "silent !". command
      execute "normal gg0"
      redraw
    endif
  endfunction
```
  
<a name="Highlighting"></a>

### Highlighting [↸](#toc)

- by Syntax Highlighting
- `syntax.vim` (in the slides folder)

```vim
  if exists("b:current_syntax")
    finish
  endif
  
  syn match Statement "\zsS\zeingle\|\zsO\zepen\|\zsL\zeiskov\|\zsI\zenterface\|\zsD\zeependency"
  
  syn match red "awesome"
  
  hi def link red Special


  syn match matchURL /http[s]\?:\/\/[[:alnum:]%\/_#.-]*/
  " hi matchURL ctermfg=14
  hi matchURL ctermfg=red
```

- some Syntax highlighting infos:
  - [Creating your own syntax files](https://vim.fandom.com/wiki/Creating_your_own_syntax_files)
  - [Match and highlight urls](https://vi.stackexchange.com/a/23338)
