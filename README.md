# nssh

一个支持密码登录 `ssh` 的脚本,支持阿里云堡垒机和 `jumpserver` 堡垒机，同时兼容原生 `SSH` 密钥登录主机。

```shell
nssh -l
Available hosts:
--------------------------------------------------
NUM   HOST ALIAS                LOGIN TYPE     
--------------------------------------------------
  1)  test-pass                 [Password/2FA]
  2)  pre                       [Password/2FA]
  3)  online                    [Password/2FA]
  4)  test                      [Native SSH]
--------------------------------------------------

Enter number to connect (or 'q' to quit): 
```

## 原生ssh登录(密钥登录)

nssh <hostname>

与原生 `SSH` 密钥登录主机完全一样。

## 密码ssh登录

nssh <hostname>

## 配置文件

### 原生配置文件 `~/.ssh/config` 即密钥登录主机的主机配置

配置示例:

```shell
Host test
  HostName 10.10.10.10
  Port 10022
  User root
```

兼容原生 `SSH` 配置文件。

### 密码登录配置文件 `~/.ssh/nssh_config`

配置示例：

```shell
# 直接使用密码登录的主机
Host test-pass
  HostName 10.10.10.11
  Port 10022
  User root
  Pass 123456
  LoginType password

# 使用密码登录jumpserver类型堡垒机
Host pre
  HostName 10.10.10.12
  Port 10022
  User root
  Pass 123456
  LoginType jumpserver

# 使用密码登录阿里云带动态验证码的堡垒机
Host online
  HostName 10.10.10.13
  Port 10022
  User root
  Pass 123456
  LoginType bastion_2fa_ali
```

配置项说明：

- Pass: 登录密码
- LoginType: 登录方式，可选值，密码直接登录：`password`、jumpserver 堡垒机：`jumpserver`、阿里云堡垒机：`bastion_2fa_ali`

因为不同的堡垒机登录方式不同，如果需要加入新的登录方式支持，需要根据服务器返回，进行自定义开发。

## 使用

1. 安装 `expect`

```shell
sudo apt-get install expect
```

2. 将本项目下的脚本放到 `/usr/local/bin` 目录（或其他存在于PATH的目录）下并赋予执行权限

```shell
chmod +x nssh
chmod +x login_password.exp
chmod +x login_jumpserver.exp
chmod +x login_bastion_2fa_ali.exp
```

3. 配置 `~/.ssh/config` 和 `~/.ssh/nssh_config` 的主机
4. 运行 `nssh <hostname>` 即可登录主机

### 其他命令：

- `nssh -h` 显示帮助信息
- `nssh -l` 显示已配置的主机列表