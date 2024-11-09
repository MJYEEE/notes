# 打造自己的nvim
选择nvim的一个最重要的原因就是因为它能够在命令行中使用，在一些没有图形化界面的时候就能够使用，比如远程ssh的时候。

这是本次配置的基础[仓库](https://github.com/MJYEEE/JYNvim)，并没有官方案例来的好看，但是胜在简单，能理解配置过程。

# 实验环境
ubuntu20.04
- nodejs : v18.20.4 (版本不能太低)
- 一个Nerd字体 (图标显示)

# nvim安装
如果只是简单体验一下nvim，可以直接用apt下载
```shell
sudo apt install neovim
```
但是这个版本太低了，由于neovim的插件很多依赖最新版本，所以这里去[github](https://github.com/neovim/neovim)下载指定版本。
我选择的是Nvim0.9.5，下载完nvim-linux64.tar.gz后，将其解压：
```shell
wget https://github.com/neovim/neovim/releases/download/v0.9.5/nvim-linux64.tar.gz # 或者手动下载
tar -zxvf nvim-linux64.tar.gz
```
解压后的nvim-linux64/bin/nvim就是我们的nvim启动程序，你可以将其移动到你想要的位置，但要注意将其添加到环境目录下，又或者创建一个软链接到/usr/bin或其他已在PATH中的目录。
我选择将软链接放到/user/bin下：
```shell
cd /usr/bin
ln -s ~/nvim-linux64/bin/nvim nvim # 前面的路径根据自己解压后的位置来 
```

输入`nvim --version`来验证nvim的安装，以及对应版本。

# nvim配置的目录
nvim在启动时会去查找`~/.config/nvim/init.lua`，将其作为程序入口，所以我们所有配置都可以全部扔到上面。
但是这样不利于后期的修改维护，所以我们要最终要把它模块化例如：
```text
nvim
├── init.lua
└── lua
    ├── config
    │   └── options.lua
    └── plugins
        └──
```
这就是大概的配置目录，我们将围绕它来进行配置

# nvim基础配置
在`~/.config/nvim/init.vim`中添加配置`nvim ~/.config/nvim/init.vim`

`vim.o` 等价于 `vim.opt` 但是没有 `vim.option`

## 行号配置
- 添加行号  `vim.opt.number = true` 
- 添加相对行号  `vim.opt.relativenumber = true`

这看个人喜好，选择哪个好，当然也可以不加行号。
行号的目的是方便定位，相对行号跳转的速度更快，但是看的很难受，刚开始用的感觉很奇怪，习惯了就号。

## Tab缩进
默认的缩进与我日常使用有较大出入，所以我换成了如下配置：
``` lua
-- Tab
vim.opt.tabstop = 4 
vim.opt.softtabstop = 4 
vim.opt.shiftwidth = 4 
vim.opt.expandtab = true 
```

## 系统剪切板
在nvim里面进行复制粘贴，它并没有复制到系统剪切板中，而是放在了nvim的剪切板中，也就是说你在nvim里面复制粘贴，系统没有复制，在nvim外面粘贴不了，所以需要配置：
- 配置 `vim.opt.clipboard = "unnamedplus"`

## 鼠标支持
在前期不能熟练使用nvim的时候，鼠标是很有必要的：
- `vim.opt.mouse = "a"`

'a' 表示全局启用鼠标支持。你可以用鼠标进行选择、滚动和调整分割窗格等操作。对于很多用户，尤其是新手来说，这可以提供更直观的编辑体验

## 分割窗口设置
在nvim中我们可以显示两个文件，或者同时显示一个文件的两个不同的位置，又或者并排比较两个文件，等等，这些可以通过分屏来实现：
- `:sp`：纵向分屏，后可跟文件名来打开指定，如果不写文件名，打开当前文件
- `:vsp`：横向分屏，后可跟文件名来打开指定，如果不写文件名，打开当前文件

切换屏幕：
- `ctrl+w h,j,k,l` 上下左右键  

默认分在上面和左侧，但是总感觉怪怪的，为了让分屏的顺序更舒服：
新分割窗口时，分别将新窗口放置在当前窗口下方和右侧
- `vim.opt.splitbelow = true` 
- `vim.opt.splitright = true`

更多分屏操作可以看 [5分钟学会Vim分屏操作方方面面](https://www.51cto.com/article/633360.html)

## 整合
因为我用nvim还没有到如火纯青的地步，所以很多设置都没有接触，所以上面就是我nvim的基础配置，后面会慢慢补上其他在使用过程中用到的配置。

目前这些基础配置都是写在init.lua中，但是随着nvim的进一步使用，配置文件会变多，所以为了方便管理，我们可以将配置文件拆分到不同的文件中：
我们创建一个lua文件夹，来存放我们所有的配置：`mkdir ./config/nvim/lua`。

因为后面要用到很多的插件，所以为了方便管理，我们将基础配置和插件配置分开管理：
```shell
mkdir ./config/nvim/lua/config  # 存储基本配置
mkdir ./config/nvim/lua/plugins # 存储插件配置
```

然后我们将前面的配置移动到config文件夹中：
```shell
cp ./config/nvim/init.vim ./config/nvim/lua/config/option.lua
```

以下是option的目前的状态：
```lua
vim.opt.clipboard = "unnamedplus" -- use system clipboard
vim.opt.mouse = 'a' -- allow the mouse to be used in Nvim

-- UI config
vim.opt.number = true -- show absolute number
vim.opt.relativenumber = true -- show relative number
vim.opt.splitbelow = true  -- open new vertical split bottom
vim.opt.splitright = true  -- open new horizontal splits right 

-- Tab
vim.opt.tabstop = 4 -- number of visual spaces per TAB
vim.opt.softtabstop = 4 -- number of spacesin tab when editing
vim.opt.shiftwidth = 4 -- insert 4 spaces on a tab
vim.opt.expandtab = true -- tabs are spaces, mainly because of python
```

但是目前的配置仍是在init.vim中，现在让我们把option.lua导入到init.vim中：
```lua
require("config.option") -- init.vim中的require会自动寻找lua文件夹下的模块
```

基础配置中除了vim.opt，还有vim.keymap.set，vim.g，vim.cmd等配置，可以根据需要自行去添加。

# nvim插件管理器-LazyVim
为了能更好的管理各个插件，我们一定不能错过LazyVim，这是当下最流行的插件管理器，可以轻松自定义和扩展我们的配置，而且响应速度非常快。

## 安装lazy.vim
我们先创建一个文件来专门配置lazyVim：
```shell
nvim ~/.config/nvim/lua/config/lazy.lua # 因为它属于管理器，就把它放在config下吧，官方例子也是这么放的 -)
```
然后在里面添加官方下载代码:
```lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)
require("lazy").setup({})
```

- 通过 `vim.fn.stdpath("data")` 获取 Neovim 配置的 数据目录（通常是 `~/.local/share/nvim/`），并将 lazy.nvim 插件的路径定义为`~/.local/share/nvim/lazy/lazy.nvim`。这个路径会用于存储 lazy.nvim 插件。
- 使用 fs_stat 函数来检查 lazypath 目录是否存在。fs_stat 是用于检查文件或目录状态的函数，返回 nil 如果文件或目录不存在。如果插件没有安装，fs_stat 会返回 nil，然后进入安装流程。所以我们只会在第一次进行安装。（这里使用了 vim.uv 或 vim.loop 这两个 Lua 库，它们是 Neovim 内部的异步 I/O 库，用于处理文件系统操作和事件循环）
- 需要安装的话就会去github上拉取最新版，然后放到lazypath中。
- `vim.v.shell_error`是一个 Vim 内建的变量，它保存上一个 shell 命令的退出状态。如果 git clone 命令执行失败，vim.v.shell_error 会不等于 0，然后输出错误信息提醒我们。
- `vim.opt.rtp:prepend(lazypath)` 会将插件路径添加到Neovim的runtimepath（rtp）。rtp 是 Neovim 查找插件、配置文件和其他资源的路径列表。通过 prepend 将插件路径放在 rtp 的开头，确保 Neovim 可以优先找到并加载 lazy.nvim。
- `require("lazy").setup({})`: 这个函数用于初始化 lazy.nvim，并加载配置文件。setup函数是lazy插件配置的核心部分。通常，你可以通过传递一个配置表来定制插件管理器的行为，我们将在后面逐步添加。

写完后，将lazy.lua文件导入到init.vim中：
```lua
require("config.lazy")
```
然后启动nvim，就会自动下载lazy.nvim了。刚开始可能会黑屏，等待一段时间就会行了。
在进入nvim后，输入`:Lazy`来检查插件是否安装成功。

# 插件安装
接下来，我们将利用Lazy来安装我们所需的插件，由于插件种类庞多，这里我只安装一些常用的功能，后续会不断更新。

- 在安装插件后，nvim目录下可以会多出一个文件 lazy-lock.json，不用管它，它是 Neovim 的 lazy.nvim 插件管理器的锁定文件，用于记录所有已安装插件的具体版本和提交记录。它确保在每次安装或更新插件时使用相同的版本，以保持开发环境的一致性

## 文档树 nvim-tree 和 文件状态 lualine.nvim
[nvim-tree官方github](https://github.com/nvim-tree/nvim-tree.lua)
[lualine官方github](https://github.com/nvim-lualine/lualine.nvim)

我们先直接在lazy.vim中修改配置
```lua
local nvim_tree_plugin = {
    "nvim-tree/nvim-tree.lua",
    version = "*",
    dependencies = {"nvim-tree/nvim-web-devicons"},
    config = function()
        require("nvim-tree").setup {}
    end
}

local lualine_plugin = {
    'nvim-lualine/lualine.nvim',
    config = function()
        require('lualine').setup()
    end
}
require("lazy").setup({nvim_tree_plugin, lualine_plugin})
```
写完后，当我们重新打开nvim，就会发现lazy正在帮我们安装nvim-tree和lualine。
如果没有发现红色的报错那就是安装完成，安装完之后，退出lazy，就会发现底下多了一个状态栏，那是lualine。
接着输入`:NvimTreeOpen`，就可以打开文件树nvim-tree了。

- 请在良好的网络环境下进行
- bug 如果图标显示有问题请参考[图标修复](https://blog.csdn.net/qq_37354060/article/details/137105910)


接下来以nvimtree为例，来介绍下上述代码：
- `"nvim-tree/nvim-tree.lua"`：插件名称，也是指这个插件在github上的地址。lazy.nvim在安装插件的时候，默认就会拉取github上的仓库代码到本地进行存放。
- `version = "*"`：指定插件的版本，这里我们指定为*，表示使用最新版本，如果以后仓库有更新，则拉去最新插件代码。
- `dependencies = {"nvim-tree/nvim-web-devicons"}`：表示依赖插件，lazy.nvim会自动下载依赖插件，这里我们指定了nvim-tree/nvim-web-devicons，表示依赖nvim-tree的图标插件。
- `config = function() ... end`：插件的配置函数，这里我们直接调用了nvim-tree的setup函数，传入一个空的配置表，表示使用默认配置。

上面我们直接在lazy.lua中配置了插件，但当我们的配置越来越多，lazy.lua就会显得非常臃肿，好在lazy支持使用plugins目录统一编排插件：
- 将lazy.lua中的setup参数变为setup("plugins")，同时移除掉其他插件安装配置的代码
- 在plugins目录中创建对应插件配置模块的lua脚本
  
nvim-tree.lua：
```lua
return {
    {
        "nvim-tree/nvim-tree.lua",
        version = "*",
        dependencies = {"nvim-tree/nvim-web-devicons"},
        config = function()
            require("nvim-tree").setup {}
        end
    }
}
```
lualine.lua：
```lua
return {
    {
        'nvim-lualine/lualine.nvim',
        config = function()
            require('lualine').setup()
        end
    }
}
```
每一个脚本模块都将返回一个table，当我们调用plugins时，会return后面的{}，每个{}都代表该插件的配置。
当我们写完配置后，重新启动nvim，没有报错并且输入`:Lazy`看到插件正常加载，就说明我们配置完成了。
现在我们的全部配置文件结构如下：
```
nvim
├── init.lua
└── lua
    ├── config
    │   └── options.lua
    |   └── lazy.lua
    └── plugins
        └── lualine.lua
        └── nvim-tree.lua
```
之后所有的插件也将按照上面的方式配置。

- 注意一个{}只能配置一个插件，如果要配置多个插件，需要创建多个{}。

## 自动补全 
自动补全要用到的插件非常多 （太多了，直接抄了[配置](https://martinlwx.github.io/zh-cn/config-neovim-from-scratch/)）

首先是 [nvim-cmp](https://github.com/hrsh7th/nvim-cmp)

我们在plugins下兴建一个nvim-cmp.lua文件`nvim nvim-cmp.lua`，内容如下：
```lua
return {
    {
      "hrsh7th/nvim-cmp",
      dependencies = {
        "lspkind.nvim",  -- 提供 LSP 类型图标 
        "hrsh7th/cmp-nvim-lsp", -- LSP 自动补全
        "hrsh7th/cmp-buffer", -- 缓冲区自动补全
        "hrsh7th/cmp-path", -- 路径自动补全
        "hrsh7th/cmp-cmdline", -- 命令行自动补全
        "L3MON4D3/LuaSnip", -- 添加 LuaSnip 作为依赖项  snippets引擎，不装这个自动补全会出问题
      },
      config = function() -- 配置
        local has_words_before = function() --  用来判断光标前是否有单词。如果没有单词（比如光标在行首），则不会触发补全。
            unpack = unpack or table.unpack
            local line, col = unpack(vim.api.nvim_win_get_cursor(0))
            return col ~= 0 and vim.api.nvim_buf_get_lines(0, line - 1, line, true)[1]:sub(col, col):match("%s") == nil
        end

        local luasnip = require("luasnip") 
        local cmp = require("cmp")

        cmp.setup({
            snippet = {
                -- REQUIRED - you must specify a snippet engine
                expand = function(args)
                    require('luasnip').lsp_expand(args.body) --设置了 luasnip 作为代码片段引擎
                end,
            },
            mapping = cmp.mapping.preset.insert({
                -- Use <C-b/f> to scroll the docs
                ['<C-b>'] = cmp.mapping.scroll_docs( -4), -- 向上滚动文档
                ['<C-f>'] = cmp.mapping.scroll_docs(4), -- 向下滚动文档
                -- Use <C-k/j> to switch in items
                ['<C-k>'] = cmp.mapping.select_prev_item(), -- 上一个补全项
                ['<C-j>'] = cmp.mapping.select_next_item(), -- 下一个补全项
                -- Use <CR>(Enter) to confirm selection
                -- Accept currently selected item. Set `select` to `false` to only confirm explicitly selected items.
                ['<CR>'] = cmp.mapping.confirm({ select = true }), -- 确认选择项

                -- A super tab
                -- sourc: https://github.com/hrsh7th/nvim-cmp/wiki/Example-mappings#luasnip
                ["<Tab>"] = cmp.mapping(function(fallback) 
                    -- Hint: if the completion menu is visible select next one
                    if cmp.visible() then
                        cmp.select_next_item() -- 如果补全菜单可见，选择下一个项
                    elseif has_words_before() then
                        cmp.complete()  -- 如果光标前有单词，触发补全
                    else
                        fallback() -- 否则调用默认行为
                    end
                end, { "i", "s" }), -- i - insert mode; s - select mode
                ["<S-Tab>"] = cmp.mapping(function(fallback)
                    if cmp.visible() then
                        cmp.select_prev_item() -- 如果补全菜单可见，选择上一个项
                    elseif luasnip.jumpable( -1) then
                        luasnip.jump( -1)  -- 跳转到前一个代码片段
                    else
                        fallback() -- 否则调用默认行为
                    end
                end, { "i", "s" }),
            }),

          -- Let's configure the item's appearance
          -- source: https://github.com/hrsh7th/nvim-cmp/wiki/Menu-Appearance
          formatting = {
              fields = { 'abbr', 'menu' }, -- 在菜单中展示补全项的简称和来源

              -- customize the appearance of the completion menu
              format = function(entry, vim_item)
                  vim_item.menu = ({
                      nvim_lsp = '[Lsp]',
                      luasnip = '[Luasnip]',
                      buffer = '[File]',
                      path = '[Path]',
                  })[entry.source.name]
                  return vim_item
              end,
          },

          -- Set source precedence
          sources = cmp.config.sources({
            { name = 'nvim_lsp' },    -- LSP 补全
            { name = 'luasnip' },     -- 代码片段补全
            { name = 'buffer' },      -- 缓冲区补全
            { name = 'path' },        -- 路径补全
          })
        })
	    end,    
    }
}
```
- 如果直接复制有大量的^M，请使用`:%s/\r//g`来去除。（这是win自带的回车符，linux下没有）

配置完之后，重启nvim，输入`:help cmp`，就会看到nvim-cmp的帮助文档。

但是现在输入`:`，会有一些基础的默认补全，但是因为没有配置lsp，无法对特定语言进行补全，下面我们来配置lsp：

新建一个lsp.lua文件`nvim lsp.lua`，内容如下：
```lua
return {
    -- mason.nvim 插件配置
    {
      "williamboman/mason.nvim",
      config = function()
        require('mason').setup({
          -- 定义 package 的安装状态的图标
          ui = {
            icons = {
              package_installed = "✓", -- 已安装
              package_pending = "➜",   -- 安装中
              package_uninstalled = "✗"  -- 未安装
            }
          }
        })
      end
    },
  
    -- mason-lspconfig 插件配置
    {
      "williamboman/mason-lspconfig.nvim", -- mason-lspconfig 插件，用于自动安装和配置 LSP 服务器
      dependencies = { "williamboman/mason.nvim" },  -- 指定依赖关系,确保在 mason.nvim 之后加载要不然总会有个信息提示。
      after = "mason.nvim",  -- 确保在 mason.nvim 之后加载
      config = function()
        -- 配置 mason-lspconfig 插件，确保所需的 LSP 服务器已安装
        require('mason-lspconfig').setup({
          -- 在这里列出你希望自动安装的 LSP 服务器
          ensure_installed = { 'pyright', 'lua_ls', 'rust_analyzer' },
        })
      end
    },
  
    -- nvim-lspconfig 插件配置
    {
      "neovim/nvim-lspconfig",
      config = function()
        -- LSP 配置部分
        local lspconfig = require('lspconfig')

        -- 设置一些常用的快捷键
        local opts = { noremap = true, silent = true }
        -- 显示诊断信息的快捷键（跳转、查看诊断信息）
        vim.keymap.set('n', '<space>e', vim.diagnostic.open_float, opts)
        vim.keymap.set('n', '[d', vim.diagnostic.goto_prev, opts)
        vim.keymap.set('n', ']d', vim.diagnostic.goto_next, opts)
        vim.keymap.set('n', '<space>q', vim.diagnostic.setloclist, opts)
  
        -- 定义一个 `on_attach` 函数，在 LSP 服务器附加到缓冲区时执行
        local on_attach = function(client, bufnr)
          -- 设置 omnifunc（用于代码补全）
          vim.api.nvim_buf_set_option(bufnr, 'omnifunc', 'v:lua.vim.lsp.omnifunc')

          -- 为缓冲区定义一些快捷键，简化常用 LSP 功能的调用
          local bufopts = { noremap = true, silent = true, buffer = bufnr }
          -- 跳转到声明、定义、查看信息等
          vim.keymap.set('n', 'gD', vim.lsp.buf.declaration, bufopts)
          vim.keymap.set('n', 'gd', vim.lsp.buf.definition, bufopts)
          vim.keymap.set('n', 'K', vim.lsp.buf.hover, bufopts)
          vim.keymap.set('n', 'gi', vim.lsp.buf.implementation, bufopts)
          vim.keymap.set('n', '<C-k>', vim.lsp.buf.signature_help, bufopts)

          -- 工作区文件夹操作
          vim.keymap.set('n', '<space>wa', vim.lsp.buf.add_workspace_folder, bufopts)
          vim.keymap.set('n', '<space>wr', vim.lsp.buf.remove_workspace_folder, bufopts)
          vim.keymap.set('n', '<space>wl', function()
            print(vim.inspect(vim.lsp.buf.list_workspace_folders()))
          end, bufopts)

          -- 代码重命名、引用、类型定义等操作
          vim.keymap.set('n', '<space>D', vim.lsp.buf.type_definition, bufopts)
          vim.keymap.set('n', '<space>rn', vim.lsp.buf.rename, bufopts)
          vim.keymap.set('n', '<space>ca', vim.lsp.buf.code_action, bufopts)
          vim.keymap.set('n', 'gr', vim.lsp.buf.references, bufopts)

           -- 格式化代码
          vim.keymap.set("n", "<space>f", function()
            vim.lsp.buf.format({ async = true })
          end, bufopts)
        end
        
        -- 配置 lspcapabilities（用于代码补全的能力）
        local capabilities = require('cmp_nvim_lsp').default_capabilities()

        -- 其他语言服务器同理
        -- 配置 Pyright 语言服务器，关联 on_attach 和 capabilities
        lspconfig.pyright.setup({
            on_attach = on_attach,
            capabilities = capabilities, 
        })
      end
    }
  }
```

在配置这块的时候出现了一点小插曲：
我一开始不是怎么写的，因为看的配置教程很杂导致弄混了，写成了如下这样：
```lua
return {
    {
      "williamboman/mason.nvim", -- mason.nvim 是一个非常方便的插件，用来管理 LSP 服务器、自动化安装、卸载等操作
      "williamboman/mason-lspconfig.nvim",
      "neovim/nvim-lspconfig",

      config = function()
        -- mason.setup 配置了安装包状态的图标，这在 Neovim 中的 UI 中会有所体现，非常直观
        require('mason').setup({
            ui = {
                icons = {
                    package_installed = "✓",
                    package_pending = "➜",
                    package_uninstalled = "✗"
                }
            }
        })
        
        require('mason-lspconfig').setup({
            -- 这行配置指定了在 Mason 中自动安装的 LSP 服务器。 
            ensure_installed = { 'pyright', 'lua_ls', 'rust_analyzer' },
        })

        local lspconfig = require('lspconfig')
        
        -- ......

        lspconfig.pyright.setup({
          on_attach = on_attach,
        })
      end
    }
}
```

--- 
按照上面这样写，插件是能够正常的下载，并启动，在lazy中不会报错，但配置的写法是错的，没法识别。

当我尝试输入`:Mason`时，却怎么都启动不起来，一开始还以为是网络配置原因，以为是插件内容有残缺，后来下了lazy官方的[样例配置](https://github.com/LazyVim/starter)，是可以用的，才发现是配置文件没写对。

lazy.nvim 对插件配置的结构有要求，尤其是定义多个插件以及插件的配置，这个的代码return了一个表（也就是一个{ }），但是同时列出多个插件 (mason.nvim，mason-lspconfig.nvim，nvim-lspconfig) 和它们的配置，然而在 lazy.nvim 中，每个插件需要作为一个单独的条目进行列出，而不是把它们放在一个{}里。

你可以return多个插件{}，但是不能把插件的配置放在一个{}，就像前面的正确的代码，添加多个表也就是多个{}，才能让lazy.nvim识别。

或者可以让 另外两个插件作为其中一个插件的dependencies，这样也算一个插件配置。但我没试过，不知道有没有其他的问题。

---

做完这些`:Mason`终于能用了，但是又有新的问题，当我新建一个py文件`nvim p.py`，准备测试一下自动补全，试了一下import，发现根本没有用。

一开始，进入Mason`:Mason` 发现pyright没有正确安装，一直下载不了，是用npm安装的，我觉得应该是网络的缘故，所以我换了个镜像源来安装，就安装成功了。
```shell
npm config set registry http://mirrors.cloud.tencent.com/npm/
```

Mason里也显示了pyright已经安装成功，但是nvim里还是没有自动补全。

于是我又去验证pyright是否安装正确，通过检查pyright-langserver的安装位置和版本号以及启动日志，发现是nodejs的问题，我的nodejs是利用apt下载的版本过低，不支持 pyright 使用的某些新语法，所以接下来我去更新了nodejs的版本，使用 Node Version Manager (NVM) 升级 nodejs 版本。
安装方法：
```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

安装完成后，重新启动终端，或者手动加载 nvm
```shell
source ~/.bashrc 
```

然后，安装并使用指定版本，我选择的是 v18.20.4（如果安装最新版的话，其他又要更新比如g++，所以我不搞了）：
```shell
nvm install v18.20.4
nvm use v18.20.4
```

之后，重新安装pyright：
```shell
npm uninstall -g pyright
npm install -g pyright
pyright --version
pyright-langserver --stdio
```

这下重新启动nvim，再次验证python的补全，终于能用了。
当然这只是python的自动补全，如果是其他的比如rust（这里我又出现了问题）

当我按照python添加rust时，发现没有连上lsp。
后来发现实验机上没装rust，所以装了rust后，重新启动nvim，还是没有。
又发现rust也要装rust_analyzer（vscode用多了，不知道咋装上的）:
```shell
rustup component add rust-analyzer
```
装完之后，创建了单个rs文件来测试，发现还是不能用，但是lsp已经连上了。
最终得知rust_analyzer只能在cargo项目文件中使用，单rs文件没用-。-

# 总结
以上便是我的基础nvim的配置过程，虽然最终配出来的东西还很简单，而且还很丑，但仅仅是配这么几个东西都配了我3天，第二天配那个自动补全还把我人配麻了。
很大原因是看的教程太多太杂，又不愿意去看官方教程，所以跌跌撞撞踩了不少坑。

后续我也会不断的把配置添加进去，打造一个更个性化都编辑器，这是本次配置的[仓库](https://github.com/MJYEEE/JYNvim)

# 参考资料
- <https://martinlwx.github.io/zh-cn/config-neovim-from-scratch/>
- <https://www.meowpass.com/pages/45c045/>
- [字体混乱解决解决](https://blog.csdn.net/qq_37354060/article/details/137105910)