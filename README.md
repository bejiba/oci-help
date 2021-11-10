# 甲骨文实例抢购教程

## 获取用户 OCID 和租户 OCID
登录甲骨文网站
1. 点击右上角头像 -> 点击类似 oracleidentitycloudservice/xxx@xx.com 的条目，进入后即可看到用户的 OCID。
2. 点击右上角头像 -> 点击类似 租户:xxx 的条目, 进入页面后即可获取租户的 OCID。

## 获取创建实例需要的参数
### 方法一
登录甲骨文网站，创建实例--保存为堆栈(Save as stack),下载 Terraform 配置，解压得到 main.tf 用文本编辑器打开即可。
### 方法二
登录甲骨文网站，创建实例，按F12打开浏览器开发者工具，点击创建实例发起请求，在开发者工具的网络中找到对应的网络请求获取参数。

## 安装并配置 OCI-Cli
### 安装 OCI-Cli
#### Linux
```
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```
#### macOS
```
brew install oci-cli
```
一路回车即可，当出现 ===> Modify profile to update your $PATH and enable shell/tab completion now? (Y/n): 时，输入y并回车。  
安装完成后执行下面命令
```
# 重新加载环境变量
. ~/.bashrc
# 查看 oci 版本
oci -v
```

### 配置 OCI-Cli
执行下面命令配置 OCI
```
oci setup config
```
当遇到下面几步时需要按要求输入相应的内容，其他步骤直接回车即可。  
> Enter a user OCID: # 输入你的用户ocid  
> Enter a tenancy OCID: # 输入你租户ocid  
> Enter a region by index or name  # 选择你帐号所在区域，输入编号回车  
> Do you want to generate a new API Signing RSA key pair? ...: y  # 输入y,并回车  

打开甲骨文网站，点击右上角头像 -> 用户设置 -> API 密钥 -> 添加API 密钥，将下面命令查看公钥的全部内容复制添加即可。
```
# 查看生成的公钥
cat ~/.oci/oci_api_key_public.pem
```
执行下面命令，检查配置是否正确。
```
oci iam availability-domain list
```

## Telegram Bot 消息提醒
如果需要通过 Telegram Bot 发送抢购实例成功的提醒，需要创建机器人获取 token 并获取用户的 id。
1. 通过 [BotFather](https://t.me/BotFather) 创建一个 Bot 获取 token。 
2. 通过 [IDBot](https://t.me/myidbot) 获取用户 id。

## 开始抢购实例
### 方法一（使用编译好的 go 可执行程序抢购实例）
根据系统类型下载对应的可执行文件 [下载地址](https://github.com/lemoex/oci-help/releases/latest)。  
解压后，执行下面命令运行程序
```
./oci-help
```
Telegram Bot 消息提醒配置：  
运行程序后，在主菜单选择第 3 项 [ Telegram Bot 消息提醒 ]，接着选择第 1 项设置 [ token 和用户 id ]。  
开始抢购实例：   
主菜单选择第 2 项 [ 新建抢购实例任务 ]，根据提示输入对应的参数即可开始抢购实例。

> 当程序意外停止或者手动终止后，运行程序后在主菜单选择第 1 项 [ 历史抢购实例任务 ] 可以查看之前添加的抢购实例任务，输入对应的序号即可开始抢购实例。

### 方法二
下载 [oci-help.sh](https://github.com/lemoex/oci-help/blob/main/shell/oci-help.sh) 脚本。
用文本编辑器打开 oci-help.sh，根据上面获取到的参数修改脚本。保存后通过下面命令运行脚本开始新建或者升级实例。
```
# 新建实例
./oci-help.sh --launch
# 升级实例
./oci-help.sh --update
```

## 使用 screen 在后台运行
### 在 screen 会话中运行程序
```
# 在 screen 中运行 oci-help
screen -S oci-help ./oci-help

# 在 screen 中运行新建实例命令
screen -S oci-help ./oci-help.sh --launch
# 在 screen 中运行升级实例命令
screen -S oci-help ./oci-help.sh --update
```

### 从 screen 会话中分离
按住 Ctrl 键不松，依次按字母 A 和 D 键，即可从当前screen中分离；或者直接关闭终端窗口。

### 重新连接 screen 会话
```
screen -r oci-help
```

### 终止 screen 会话
在 screen 会话中 按下 Ctrl 不松，按下字母 D 键，即可终止该 screen 会话。



### 以下为我自己摸索安装指定oci 3.1.1版本；
#常规升级
sudo apt update   
sudo apt -y upgrade

以下＃号为python创建和配置虚拟环境
#sudo apt install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
#wget https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tgz
#tar -xf Python-3.8.3.tgz
#cd Python-3.8.3
#./configure --enable-optimizations
#sudo make altinstall
#通过运行以下命令之一来创建虚拟环境
#python3.8 -m venv oracle-cli
#通过运行以下命令激活虚拟环境
#source oracle-cli/bin/activate
#下载并解压oci-cli.zip。
#运行以下命令。
#pip install oci_cli-*-py2.py3-none-any.whl



如果系统自带python2 
查看python的版本
直接输入python就能看到python的版本
先要卸载python2安装python3
卸载python2
sudo apt-get remove python


安装python3
sudo apt-get install python3
sudo apt-get autoremove
此时系统中python2已经被删除，python3已经安装，但是默认的python、pip处理没有更新，
所以
更新python3为默认python
cd /usr/bin

rm -rf python

ln -s /usr/bin/python3 python

将pip3设置为默认pip

ln -s /usr/bin/pip3 /usr/bin/pip

使用以下命令为Python 3及其所有依赖项安装pip
apt install -y python3-pip
升级pip
pip install --upgrade pip
pip3 install --upgrade pip
通过发出以下命令来验证安装，该命令将打印pip版本：
pip --version

下载3.1.1并解压缩
sudo apt -y install unzip
wget https://github.com/oracle/oci-cli/releases/download/v3.1.1/oci-cli-3.1.1.zip
unzip oci-cli-3.1.1.zip
cd /root/oci-cli/
pip install oci_cli-3.1.1-py3-none-any.whl
pip3 install oci_cli-3.1.1-py3-none-any.whl
pip install oci-cli

遇到的问题可执行下列命令
pip3 install --upgrade pip
pip install --ignore-installed PyYAML


# 重新加载环境变量
. ~/.bashrc
# 查看 oci 版本
oci -v

卸载
pip uninstall oci-cli
