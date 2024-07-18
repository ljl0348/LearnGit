### 1、安装基本工具

- wsl中输入命令安装 zsh：
```bash
sudo apt install zsh
```

- 设置默认终端为zsh
```bash
chsh -s /bin/zsh
```
### 2、安装oh-my-zsh

- wsl或远程终端中输入命令安装，其中国内curl镜像地址：
```bash
sh -c "$(curl -fsSL https://gitee.com/pocmon/ohmyzsh/raw/master/tools/install.sh)"
```

### 3、安装插件

#### 3.1、zsh -autosuggestions插件
zsh -autosuggestions是一个命令提示插件，当你输入命令时，会自动推测你可能需要输入的命令，按下右键可以快速采用建议。
-  wsl或远程终端中输入命令安装：
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
#### 3.2、zsh-syntax-highlighting插件
zsh-syntax-highlighting是一个命令语法校验插件，在输入命令的过程中，若指令不合法，则指令显示为红色，若指令合法就会显示为绿色。
-  wsl或远程终端中输入命令安装：
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
#### 3.3、启用插件
- 修改`~/.zshrc`中插件列表：
	- -可以输入 `vi ~/.zshrc` 利用vim编辑器编辑文件，按`i`进入编辑模式
	- 编辑完成后，按`Esc` 切换至命令模式，再按`:`进入末行模式，输入`wq`保存并退出文件。 
	- 开启新的 Shell 或执行 `source ~/.zshrc`，就可以开始体验插件。
```bash
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```


### 安装插件后命令：

`Ctrl + e` 补全提示命令