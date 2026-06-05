---
title: Nginx 基础配置与常用技巧
published: 2025-07-08
description: '本文总结了 Nginx 的核心配置方法、反向代理设置以及常见的性能优化技巧，适合快速查阅。'
image: ''
tags: ["nginx", "运维", "服务器", "反向代理"]
category: '运维笔记'
draft: false
---

# Linux

以下是一些常用的 **Nginx** 相关命令，适用于 Linux/macOS 系统：

------

### **1. 启动/停止/重启 Nginx**

```bash
# 启动
sudo systemctl start nginx    # Systemd 系统
sudo service nginx start      # SysVinit 系统

# 停止
sudo systemctl stop nginx
sudo service nginx stop

# 重启
sudo systemctl restart nginx
sudo service nginx restart

# 平滑重启（重新加载配置，不中断服务）
sudo systemctl reload nginx
sudo service nginx reload
```

------

### **2. 检查 Nginx 配置**

```bash
# 测试配置文件语法是否正确
sudo nginx -t

# 测试配置并显示文件路径
sudo nginx -T
```

------

### **3. 查看 Nginx 状态**

```bash
# 查看运行状态
sudo systemctl status nginx
sudo service nginx status

# 查看进程
ps aux | grep nginx
```

------

### **4. 设置开机启动**

```bash
sudo systemctl enable nginx   # Systemd 系统
sudo chkconfig nginx on       # SysVinit 系统（如 CentOS 6）
```

------

### **5. 查看 Nginx 版本**

```bash
nginx -v      # 简要版本
nginx -V      # 详细版本（含编译参数）
```

------

### **6. 查看 Nginx 日志**

```bash
# 默认访问日志
tail -f /var/log/nginx/access.log

# 默认错误日志
tail -f /var/log/nginx/error.log
```

------

### **7. 其他实用命令**

```bash
# 强制停止（立即终止）
sudo pkill -9 nginx

# 检查 Nginx 监听的端口
sudo netstat -tulnp | grep nginx
# 或
sudo ss -tulnp | grep nginx
```

------

### **8. 配置文件路径参考**

- 主配置文件：`/etc/nginx/nginx.conf`
- 站点配置目录：`/etc/nginx/conf.d/` 或 `/etc/nginx/sites-enabled/`
- 默认网站根目录：`/usr/share/nginx/html` 或 `/var/www/html`

------

### **注意事项**

1. 执行命令可能需要 `sudo` 权限。
2. 修改配置后，务必用 `nginx -t` 测试语法，再 `reload` 生效。
3. 路径可能因系统或安装方式不同而变化（如通过源码编译的 Nginx）。

如果有具体问题，可以补充说明场景哦！

# windows

启动后，直接在浏览器输入：

```powershell
locahost:80
```

就可以看到nginx的页面，证明启动成功。

在 Windows 系统上，Nginx 的操作与 Linux 有所不同，主要依赖于命令行（CMD）或 PowerShell。以下是常用的 **Nginx Windows 版** 相关命令：

------

### **1. 启动/停止/重启 Nginx**

```powershell
:: 进入 Nginx 安装目录（根据实际路径调整）
cd C:\nginx

:: 启动 Nginx（直接运行可执行文件）
start nginx（推荐命令，不会卡主终端或者直接双击exe文件一次即启动一个，点击多次会启动多个）
或者
.\nginx.exe（不推荐，会卡主终端）

:: 停止 Nginx（快速终止）
nginx -s stop

:: 优雅停止（处理完当前请求后退出）
nginx -s quit

:: 重新加载配置（不中断服务）
nginx -s reload

:: 重新打开日志文件（日志切割后使用）
nginx -s reopen
```

------

### **2. 检查 Nginx 配置**

```powershell
:: 测试配置文件语法是否正确
nginx -t

:: 测试配置并显示文件路径
nginx -T

nginx -T输出：

PS D:\nginx-1.28.0> .\nginx.exe -T
nginx: the configuration file D:\nginx-1.28.0/conf/nginx.conf syntax is ok
nginx: configuration file D:\nginx-1.28.0/conf/nginx.conf test is successful
# configuration file D:\nginx-1.28.0/conf/nginx.conf:

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;
```

------

### **3. 查看 Nginx 进程**

```powershell
:: 查看 Nginx 进程
tasklist /fi "imagename eq nginx.exe"

:: 强制终止所有 Nginx 进程
taskkill /f /im nginx.exe
```

------

### **4. 查看 Nginx 版本**

```powershell
nginx -v      # 简要版本
nginx -V      # 详细版本（含编译参数）
```

------

### **5. 查看日志**

- 日志文件默认位于 Nginx 安装目录的

  ```
  logs
  ```

   文件夹下：

  - 访问日志：`logs/access.log`
  - 错误日志：`logs/error.log`

- 使用记事本或终端查看：

  ```powershell
  type logs\error.log
  ```

------

### **6. 配置文件路径**

- 主配置文件：`conf/nginx.conf`
- 默认网站根目录：`html\`

------

### **7. 设置开机自启动**

#### **方法 1：通过计划任务**

1. 按 `Win + R` 输入 `taskschd.msc` 打开任务计划程序。
2. 创建任务，触发器设置为“登录时”，操作为启动 `nginx.exe`（需填写完整路径）。

#### **方法 2：注册为服务（需第三方工具）**

使用工具如 [NSSM](https://nssm.cc/) 将 Nginx 注册为 Windows 服务：

```powershell
nssm install nginx "C:\nginx\nginx.exe"
nssm start nginx
```

------

### **注意事项**

1. **路径问题**：所有命令需在 Nginx 安装目录下执行，或指定完整路径（如 `C:\nginx\nginx -s reload`）。
2. **端口冲突**：如果 80 端口被占用（如 IIS），需修改 `conf/nginx.conf` 中的 `listen` 端口。
3. **管理员权限**：部分操作需以管理员身份运行 CMD/PowerShell 。

------

### **常见问题**

- **启动失败**：检查 `error.log`，常见原因是端口冲突或配置文件语法错误。
- **如何后台运行**：直接运行 `start nginx` 会后台启动，关闭终端不会影响进程。

如果有具体报错或需求，可以进一步说明！

# windows杀死所有nginx进程

```powershell
taskkill /f /im /t nginx.exe
```
