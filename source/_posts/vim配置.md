---
title: Vim配置
date: 2016-11-13 10:25:50
tags:
- vim
---
对vim进行界面配置，以适应不同开发者的开发需求。
<!-- more -->
## 基本配置
你只要在Home目录下创建一个~/vimrc文件就可以配置vim了，如果想要所有用户生效，请修改/etc/vimrc。
###常用配置命令
~~~
set nocompatible " 关闭 vi 兼容模式
syntax on " 自动语法高亮
colorscheme molokai " 设定配色方案
set number " 显示行号
set cursorline " 突出显示当前行
set ruler " 打开状态栏标尺
set shiftwidth=4 " 设定 << 和 >> 命令移动时的宽度为 4
set softtabstop=4 " 使得按退格键时可以一次删掉 4 个空格
set tabstop=4 " 设定 tab 长度为 4
set nobackup " 覆盖文件时不备份
set autochdir " 自动切换当前目录为当前文件所在的目录
filetype plugin indent on " 开启插件
set backupcopy=yes " 设置备份时的行为为覆盖
set ignorecase smartcase " 搜索时忽略大小写，但在有一个或以上大写字母时仍保持对大小写敏感
set nowrapscan " 禁止在搜索到文件两端时重新搜索
set incsearch " 输入搜索内容时就显示搜索结果
set hlsearch " 搜索时高亮显示被找到的文本
set noerrorbells " 关闭错误信息响铃
set novisualbell " 关闭使用可视响铃代替呼叫
set t_vb= " 置空错误铃声的终端代码
" set showmatch " 插入括号时，短暂地跳转到匹配的对应括号
" set matchtime=2 " 短暂跳转到匹配括号的时间
set magic " 设置魔术
set hidden " 允许在有未保存的修改时切换缓冲区，此时的修改由 vim 负责保存
set guioptions-=T " 隐藏工具栏
set guioptions-=m " 隐藏菜单栏
set smartindent " 开启新行时使用智能自动缩进
set backspace=indent,eol,start
" 不设定在插入状态无法用退格键和 Delete 键删除回车符
set cmdheight=1 " 设定命令行的行数为 1
set laststatus=2 " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\ 
" 设置在状态行显示的信息
set foldenable " 开始折叠
set foldmethod=syntax " 设置语法折叠
set foldcolumn=0 " 设置折叠区域的宽度
setlocal foldlevel=1 " 设置折叠层数为
" set foldclose=all " 设置为自动关闭折叠 
" nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR>
" 用空格键来开关折叠
~~~
### 一键执行
很多时候完成代码后都要返回命令行执行，有时候会很麻烦，这时一键执行就实用很多。
~~~
map <F9> :call CompileRunGcc()<CR>
func! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!g++ % -o %<"
        exec "!time ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -o %<"
        exec "!time ./%<"
    elseif &filetype == 'java'
        exec "!javac %"
        exec "!time java %<"
    elseif &filetype == 'sh'
        :!time bash %
    elseif &filetype == 'python'
        exec "!time python2.7 %"
    elseif &filetype == 'html'
        exec "!firefox % &"
    elseif &filetype == 'mkd'
        exec "!~/.vim/markdown.pl % > %.html &"
        exec "!firefox %.html &"
    endif
endfunc
~~~
另外一种方法：
`au BufRead *.py map <buffer> <F9> :w<CR>:!/usr/bin/env python % <CR>`
## 插件管理与Vundle
插件的管理是vim配置中非常重要的一个内容，因为单纯的修改.vimrc并不能完全满足我们的需求，这时候我们就需要插件了。而在插件管理上现在最常用的有Vundle，pathon等。这里我们暂时只记录Vundle插件管理工具的配置过程。
### Vundle安装
~~~
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
~~~
### 配置
将以下的命令置入你的.vimrc文件中，可以根据注释移除你不想要的插件。
~~~
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
~~~
### 安装插件
在Vim的命令行运行`:PluginInstall`
也可以在shell的命令行运行:`vim +PluginInstall +qall`
## 插件的推荐
### NERD Tree
NERD Tree是一个树形目录插件，方便浏览当前目录有哪些目录和文件。
在~/.vimrc文件中配置NERD Tree，设置一个启用或禁用NERD Tree的键映射
~~~
"F5开启关闭树"
nmap <F5> :NERDTreeToggle<cr>
let NERDTreeChDirMode=1
"显示书签"
let NERDTreeShowBookmarks=1
"设置忽略文件类型"
let NERDTreeIgnore=['\~$', '\.pyc$', '\.swp$']
"窗口大小"
let NERDTreeWinSize=25
~~~
所以可以通过F5快捷启动或关闭树形目录。
NERD Tree提供一些常用快捷键来操作目录：

通过hjkl来移动光标
o打开关闭文件或目录，如果想打开文件，必须光标移动到文件名
t在标签页中打开
s和i可以水平或纵向分割窗口打开文件
p到上层目录
P到根目录
K到同目录第一个节点
P到同目录最后一个节点

### 代码折叠
很多的IDE都提供对方法和类进行折叠的手段，只显示定义而不显示全部代码。
我们可以在.vimrc中添加下面的代码开启功能:
~~~
" Enable folding
set foldmethod=indent
set foldlevel=99
" Enable folding with the spacebar
nnoremap <space> za
~~~
这样空格键可以折叠了。
我们还可以使用插件SimplyFold。
在.vimrc中加入下面的代码，通过Vundle进行安装:
`Plugin 'tmhedberg/SimpylFold'`
如果想要看到折叠代码的文档字符串
`let g:SimpylFold_docstring_preview=1`



