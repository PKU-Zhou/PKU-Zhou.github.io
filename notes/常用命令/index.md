---
title: 常用命令
parent: 我的笔记
nav_order: 1
has_children: false
---

*记录日常使用的一些命令*

# Git 相关

## Git 配置SSH-Key
### 1. 查看/生成 ssh-key
```bash
cd ~/.ssh
ssh-keygen -t rsa -C "zch.zhou@outlook.com" 
```
然后一直按 "Enter" 键

### 2. 获取 ssh-key 公钥
```bash
// 进入ssh目录
cd ~/.ssh
// 查看ssh 公钥  进行复制
cat id_rsa.pub
```
### 3. GitHub设置中添加公钥
点击头像 --> settings --> SSH and GPG keys --> New SSH key 

在 Title 框中，随意取一个名字，建议是使用公钥的设备名，如:EDA01

在 Key 方框中，粘贴刚才复制的公钥

### 4. 检查是否设置成功
```bash
ssh -T git@github.com
```
出现 successfully 字样即为成功

### 5. 使用 ssh 链接进行 git 操作
```bash
// 示例
git clone git@github.com:PKU-Zhou/PKU-Zhou.github.io.git
```




# 服务器相关

## 远程服务器全局代理使用本地网络转发

假设内网内的某个远程主机没有翻墙能力，但是本地机器已经配置了代理（例如使用clash，默认端口7890），可以通过端口转发使得远程主机使用本地机器的网络。

### 本地运行
```bash
ssh -N -R 20000:127.0.0.1:7890 remote_id@remote_ip
```

### 远程主机上运行
```bash
// 设置代理，注意此处的端口号与上面要一样
export http_proxy="socks5h://127.0.0.1:20000"
export https_proxy="socks5h://127.0.0.1:20000"
```

如果要取消代理:
```bash
unset http_proxy
unset https_proxy
```


# TCL 脚本语言

