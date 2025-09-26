# Linux

## apt

包管理软件，ubuntu 用。

## systemctl

systemctl 是 Linux 中的系统服务管理工具，它属于 systemd（Linux 的初始化系统和服务管理器）。

常用命令举例（以 nginx 为例）：

```bash
systemctl(系统服务管理工具)
├── start nginx      # 启动 nginx
├── stop nginx       # 停止 nginx
├── enable nginx     # 设置 nginx 开机自启动
├── restart nginx    # 重启 nginx
└── status nginx     # 查看 nginx 运行状态
```

## chown

chown = change owner，作用是改变文件或目录所属的用户和所属用户组。

示例：

```bash
chown -R ubuntu:ubuntu /var/www/html/order-badminton
```

## chmod

chmod = change mod，作用是改变文件或目录的模式（权限）。

文件和目录有三种权限：

* 读：r = 1
* 写：w = 2
* 执行：x = 4

Linux 每个文件有三组权限：

```bash
-rw-r-r-
 ↑  ↑ ↑
 |  | └── other（其他用户权限）
 |  └──── group（用户组权限）
 └─────── owner（文件所有者权限）
```

命令示例：

```bash
chmod 755 index.html

# 第一个数字对应文件所有者的权限，7 = 4 + 2 + 1（执行+写+读）。
# 后面两个数字分别对应用户组权限和其他用户权限。
```

## 用户和用户组

创建用户命令：

```bash
sudo adduser kkat
```

创建一个新用户 kkat 的时候，Linux 会自动为这个用户创建一个与用户名相同的主组。
