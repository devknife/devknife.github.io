---
layout: post
title:  "vi용 Terraform AutoComplete Plugin"
date:   2018-08-14 13:25:00
categories: terraform
---

현재까지 공식 지원하는 terraform editor plugin은 intellij용 밖에 없다.
intellij 에서는 편집만 되고 실행은 해볼수가 없으므로 vi 환경에서 auto complete 을 사용할 수 있도록 따로  plugin 을 설치해야 한다.
* 설치하려는 plugin은 여기에 -> https://github.com/juliosueiras/vim-terraform-completion
* plugin 설치는 vim-plug(https://github.com/junegunn/vim-plug) 를 이용해서 설치하면 된다.

### 설치 방법 ###
#### (1) vim-plug 설치 ####
~/.vim/autoload 디렉토리(없으면 만들고) 에서 
```bash
[ec2-user@localhost autoload] $ wget https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
[ec2-user@localhost autoload]$ pwd
/home/ec2-user/.vim/autoload
[ec2-user@localhost autoload]$ ls
plug.vim
[ec2-user@localhost autoload]$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```
#### (2) ~/.vimrc 파일을 열어 (없으면 만들기) 다음과 같이 설정 ####
```bash
" Minimal Configuration
set nocompatible
syntax on
filetype plugin indent on
 
call plug#begin('~/.vim/plugged')
 
" (Optinal) for Tag Sidebar
" Plug 'majutsushi/tagbar'
 
Plug 'hashivim/vim-terraform'
Plug 'vim-syntastic/syntastic'
Plug 'juliosueiras/vim-terraform-completion'
call plug#end()
 
" Syntastic Config
" set statusline+=%#warningmsg#
" set statusline+=%{SyntasticStatuslineFlag()}
" set statusline+=%*
 
let g:syntastic_mode_map = { 'mode': 'passive', 'active_filetypes': [],'passive_filetypes': [] }
nnoremap <C-w>E :SyntasticCheck<CR> :SyntasticToggleMode<CR>
 
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0
let g:terraform_align=1
let g:terraform_fold_sections=1
let g:terraform_remap_spacebar=1
let g:terraform_commentstring='//%s'
 
" (Optional)Remove Info(Preview) window
set completeopt-=preview
 
" (Optional)Hide Info(Preview) window after completions
autocmd CursorMovedI * if pumvisible() == 0|pclose|endif
autocmd InsertLeave * if pumvisible() == 0|pclose|endif
 
" (Optional) Enable terraform plan to be include in filter
let g:syntastic_terraform_tffilter_plan = 1
 
" (Optional) Default: 0, enable(1)/disable(0) plugin's keymapping
let g:terraform_completion_keys = 1
 
" (Optional) Default: 1, enable(1)/disable(0) terraform module registry completion
let g:terraform_registry_module_completion = 0
```

#### (3) vi를 실행하고 명령 모드에서 :PlugInstall 을 실행하면 .vimrc 에 설정한 plugin들이 자동으로 설치된다. ####
![image2018-8-14_13-12-41](https://user-images.githubusercontent.com/47875462/53388025-f7804100-39cc-11e9-8f27-8940ae216237.png)

### 사용방법 ###
vi로 terraform 파일을 열고 입력 모드에서 Ctrl+X, Ctrl+O를 누르면 자동 완성 목록이 나온다
의존성 라이브러리 중에 vim-terraform도 있으므로 편집 중에 명령모드에서 :TerraformFmt 를 실행하면 자동 포맷팅도 된다!


