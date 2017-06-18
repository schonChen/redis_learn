# redis_learn
mac redis install

# Homebrew install 
## Homebrew 是用 ruby 写的，github 存放信息
$ ruby -e "$(curl -fsSLk https://gist.github.com/raw/323731/install_homebrew.rb)"
brew 安装的软件存放在 /usr/local/Cellar 中，同时会在 /usr/local/bin, /usr/local/sbin, /usr/local/lib 中创建链接。你可能需要将 /usr/local/sbin 添加到搜索路径中。
$ vim ~/.profile

PATH="$PATH:/usr/local/sbin"
export PATH
## 通用脚本：
- 软件更新
$ brew update  
- 软件搜索
$ brew search redis
$ brew search jpg
$ brew search /^libjp[e]?g/ # 正则表达式搜索
- 查看软件信息
$ brew info redis
- 浏览软件主页
$ brew home redis
- 安装软件
$ brew install redis
- 删除软件
$ brew uninstall redis
- 查看已经安装的软件
$ brew list
- 查看某软件的安装文件
$ brew list redis
- 查看某软件的 brew 安装脚本
$ brew cat redis
- 删除所有软件升级后遗留的旧版本
$ brew cleanup 
