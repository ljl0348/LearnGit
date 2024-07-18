### 1、neovim(nvim)安装
### 1.1、检查当前是否安装nvim
- 输入命令`nvim -v` 
	 - 如果已经安装过nvim，则结果如下：
		```bash
		nvim -v
		```
		```
		NVIM v0.10.0
		Build type: RelWithDebInfo
		LuaJIT 2.1.1713484068
		Run "nvim -V1 -v" for more info
		```
		`NVIM v0.10.0` 表示版本号为v0.10.0。
		当前nvim的最新版本可以登陆官方github发布页面查看：[Releases · neovim/neovim (github.com)](https://github.com/neovim/neovim/releases)
		- 如果需要安装其他版本nvim，则需先运行`sudo apt remove neovim` 命令卸载旧版本。
### 1.2、安装nvim
- 方式1：从Ubuntu 软件仓库中安装 *(不推荐)*
	```bash
	sudo apt install neovim
	```
	这种方法在安装某些软件时，尤其是像Neovim这样快速迭代的软件时，**可能无法获取到最新版本**。  （**Ubuntu仓库中的软件包通常在Ubuntu版本发布时被锁定**，并且在该版本的生命周期内只接受安全更新和关键修复，意味着***最新的软件版本可能不会立即出现在APT仓库中***）  
----
- 方式2：通过下载官方GitHub仓库中的压缩包来进行安装
	- 访问Neovim的GitHub发布页面：[Releases · neovim/neovim (github.com)](https://github.com/neovim/neovim/releases)
	- 选择适合你系统的最新版本压缩包下载链接(Linux版本，即`nvim-linux64.tar.gz` 版本)，输入命令`wget http://....`下载
	示例：
	```bash
	wget https://github.com/neovim/neovim/releases/download/nightly/nvim-linux64.tar.gz
	```
	
	- 解压下载的文件
	```bash
	tar xzvf nvim-linux64.tar.gz
	```
	- 清理压缩包文件
	```bash
	rm -rf nvim-linux64.tar.gz
	```
	- 移动文件位置
	```bash
	sudo mv nvim-linux64 /usr/local/nvim
	```
	- 创建软链接
	```bash
	sudo ln -s /usr/local/nvim/bin/nvim /usr/bin/nvim
	```
	- 查看软件安装结果
	```bash
	 nvim -v
	```
	```
	 NVIM v0.11.0-dev-427+ga553b3687
	 Build type: RelWithDebInfo
	 LuaJIT 2.1.1720049189
	 Run "nvim -V1 -v" for more info
	```

### 1.3、设neovim为默认编辑器
-  在 WSL 中设置 Neovim 为默认编辑器需要将 `EDITOR` 和 `VISUAL` 环境变量设置为 `nvim`：
	```bash
	echo 'export EDITOR=nvim' >> ~/.zshrc
	echo 'export VISUAL=nvim' >> ~/.zshrc
	```
	- 注：已进行过Ubuntu终端美化-zsh则使用上面命令，若使用的是Bash，则将`zshrc` 改为`bashrc` 
-  验证设置是否生效
	- 设置完成后，重新打开一个终端或者重新加载你的 Shell 配置文件，然后可以使用以下命令来验证 Neovim 是否已经成功设置为默认编辑器：
	```bash
	➜  ~ echo $EDITOR
	nvim
	➜  ~ echo $VISUAL
	nvim
	```
### 1.4 命令别名
- 输入`nvim ~/.zshrc` 打开`.zshrc`文件
- 按`G`光标跳转至文本末端，再按`o`另起新行并进入插入模式。
- 添加命令 `alias vim='nvim'` 至文末，保存并退出nvim。
- 分别输入
```shell
vim --version
```
和
```shell
nvim --version
```
如果两次输出结果都为nvim的版本号，则更改成功，此后无论输入`vim` 或 `nvim` 都将进入nvim。

----
### 2、nvim相关插件安装
### 2.1、插件管理器vim-plug
- 作用：插件管理器，用于管理和安装其他插件。
- 安装：终端中输入以下命令（**该链接对应于Neovim版本的vim-plug）：
	```bash
	sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
	```
	这个命令会将 Vim-plug 下载并安装到 Neovim 的插件目录中
	**注：如果安装的是vim对应链接的vim-plug，则在使用nvim进行插件配置时会出现`not an editor command: PlugUpdate` 的错误。*
### 2.2、插件配置
-  先在终端中输入以下命令以防配置中存在部分插件安装失败：
	```bash
	pip3 install pyright
	```
-  将配置文件导入到 `~/.config/nvim` 中(配置文件可联系部门主管或其他同事进行拷贝。)
-  进入目录`~/.config/nvim`
	```bash
	cd ~/.config/nvim
	```
-  用nvim打开`init.vim`
	```bash
	nvim init.vim
	```
-  第一次进入时可能会报错，此时可以一直按住`Enter`或者下箭头，直到看到输入`Press Enter .....` 后，点击`Enter` 进入文档界面。
- 进入底行命令模式进行插件配置：
	- 先按`Esc`确保当前处于正常模式，然后按`:`进入底行模式，输入以下指令执行插件配置：
		```bash
		PlugUpdate
		```
- 配置过程中根据提示可能需要继续按`Enter`。
### 2.3、键盘映射
- 目标：将Windows系统键盘的“CapsLock”设置为“Esc”
- 作用：vim中常用`Esc`和`Ctrl`，但分布在键盘的左上和左下，经常使用会不方便，而`Caps Lock`在vim中不常用，且其功能可以用其他方式实现，如`shift + n`=`N`，或选中文本后按下`Ctrl + u`切换字符大小写。
- 方法：通过注册表编辑
	1. 打开注册表编辑器
		-  按下 `Win + R` 组合键打开运行对话框。
		- 输入 `regedit` 并按下 Enter 键打开注册表编辑器。
	2. 在注册表编辑器中，导航至以下路径：
	```arduino
	HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout
	```
	3. 创建新的二进制数值
		- -在右侧窗格中，右键点击空白处，选择 `新建` -> `二进制值`。
		- 将新创建的值命名为 `Scancode Map`
	4. 编辑 Scancode Map 值
		- 双击 `Scancode Map` 值以编辑它。
		- 输入以下十六进制数值数据来映射 CapsLock 键为 Esc 键：
		```
		00 00 00 00 00 00 00 00 02 00 00 00 01 00 3A 00 00 00 00 00

		```
	5.  重启计算机以使注册表更改生效
### 3、Vim操作
#### 3.1、进入Vim
- 输入`vim <filename>` ，可进到vim界面，此时处于正常模式 。
- 输入`vim <filename> +n` ，打开filename，并将光标移动到第n行。
### 3.2、Vim常用的三种模式及切换
#### 3.2.1、正常模式（命令模式）
- 插入模式----->正常模式：按`Esc`
- 底行模式----->正常模式：按`Esc` ；另外底行模式下执行完一条命令后会自动切换至命令模式
#### 3.2.2、插入模式
只有在该模式下才可以做文字输入。
- 正常模式----->插入模式：
	- 输入`i`      在当前光标位置进入插入模式
	- 输入`a`     在当前光标位置之后进入插入模式
	- 输入`o`     在当前行之后**插入一个新行并进入插入模式**。

- 底行模式----->插入模式：需要先进入到正常模式`Esc`，再按`i`或`a`或`o`进入。
#### 3.2.3、底行模式
- 正常模式----->底行模式：按`:` ，即按`shift + ;`
- 插入模式----->底行模式：需要先进入到正常模式`Esc` ，再按`:`进入。
### 3.3、正常模式下命令集
#### 3.3.1、光标移动
-  `G` ：移动到文章最后
- `nG` : 移动到文章第n行的行首
- `gg` ：移动到文本开始
- `$` ：移动到光标所在行的“行尾”
- `^` = `0`：移动到光标所在行的“行首”
- `w` ：光标跳到下个字的开头
-  `e` ：光标跳到下个字的字尾
- `b`  ：光标回到上个字的开头
- `nl` ：光标移到该行的第#个位置，如：5l,56l
- `gg` ：进入到文本开始
- `shift + g` = `G` ：进入文本末端
- `Ctrl + b`：屏幕往“后”移动一页
- `Ctrl + f`：屏幕往“前”移动一页
- `Ctrl + u`：屏幕往“后”移动半页
- `Ctrl + d`：屏幕往“前”移动半页
- `Ctrl + g` 列出光标所在行的行号。
#### 3.3.2、删除文字
- `x`（小写）：每按一次，删除光标所在位置的一个字符
- `nx`：例如，`6x` 表示删除光标所在位置的“后面（包含自己在内）”6个字符
- `X` （大写）：每按一次，删除光标所在位置的“前面”一个字符 （向前）
- `nX`：例如，`20X` 表示删除光标所在位置的“前面”20个字符（向前）
- `dd` ：删除光标所在行  （剪切）
- `ndd` ：从光标所在行开始删除n行 （剪切）
#### 3.3.3、复制
- `yw` ：将光标所在之处到字尾的字符复制到缓冲区中。
- `nyw` ：复制n个字到缓冲区。
- `yy` ：复制光标所在行到缓冲区。
- `nyy` ：例如，`6yy` 表示拷贝从光标所在的该行“往下数”6行文字。
- **`p`：将缓冲区内的字符贴到光标所在位置。**(包括`dd` 和`ndd`删除的内容)
- `np` ：将缓冲区内的字符贴到光标所在位置n次。

#### 3.3.4、替换
- `r` ：替换光标所在处的字符
	- 示例： 输入`rb`，则将所在处字符替换为b
	- 示例：输入`nrb`，则将所在处及之后的共n个字符替换为b
- `R` ：替换光标所到之处的字符，直到按下`Esc`键为止。
	- 输入`R` 之后会进入REPLACE模式
- `~`波浪号：将文本进行大小写转换
	- `n~` ： 将所在位置及之后共n个字符进行大小写转换

#### 3.3.5、撤销上一次操作
- `u` ：撤销上一次操作，可以多次使用；全部撤销后vim下方提示“Already at oldest change”
- `Ctrl + r` ：恢复撤销，可多次使用；全部恢复完成后vim下方提示“Already at newest change”

#### 3.3.6、更改
- `cw` ： 更改光标所在处到字尾
	- 直接进入insert模式，同时删除所处到字尾的字符。
- `cnw` ： 更改光标所在处n个字

#### 3.3.7、查找文本
- `/文本`：在当前文件中向下查找匹配的文本。按下回车后，光标将定位到第一个匹配的位置。***（光标位置下的第一个）***
- `?文本`：在当前文件中向上查找匹配的文本。按下回车后，光标将定位到第一个匹配的位置。***（光标位置前的第一个）***
	- `/文本`和`?文本` 定位到位置后，按下 n 键，可继续向下查找下一个匹配项。  按下 N 键，可向上查找前一个匹配项。
- `*`：按下`*` 键，在当前文件中向下查找当前光标所在位置的**单词**。
- `#`，按下 # 键，在当前文件中向上查找当前光标所在位置的**单词**。
#### 3.3.8、退出vim
- `ZZ` = `shift + z + z`：保存退出
- `ZQ = shift + z + q`：不保存退出
- `Ctrl + z`  
注：以上三条命令在正常模式下使用。

### 3.4、底行模式下命令集
在使用底行模式命令前，确保处于正常模式下，可按`Esc`切换，再按`:`进入底行模式。
- `set nu`：在文件中的每一行前面列出行号。
- `set nonu` ：取消显示行号。
- `n` ：`n` 表示一个数字，在冒号后输入一个数字，再按回车键就会跳到该行。
	- 示例：`:10` 输入数字10，再回车，就会跳到文章的第10行。
- `/文本` 与 `?文本`的使用，3.3.7的使用方法一致
- `%s/<查找内容>/<替换内容>/g`：全局替换命令，将当前文件中所有匹配 <查找内容> 的文本替换为 <替换内容>。
         `%s`：表示对整个文件进行替换。  
         `/g`：表示全局替换，即一行中多次出现的匹配都会被替换。
- `w`: 在冒号输入字母`w`就可以将文件保存起来
- `q`：按`q`就是退出，如果无法离开vim，可以在`q`后跟一个`!` 强制离开vim。
- `wq`：一般建议离开时，搭配`w`一起使用，这样在退出的时候还可以保存文件
	- **注： vim可以打开一个不存在的文件，如果q退出，不创建文件，如果wq退出，创建文件。**
- `!cmd`: 直接在不退出vim的情况下执行命令，进行查看、编译、运行等动作。