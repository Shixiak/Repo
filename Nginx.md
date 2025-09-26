# Nginx 使用

## 安装

可以通过 `nginx -v` 查看系统中是否存在 nginx。如果不存在需要安装 nginx。

```bash
sudo apt update
sudo apt install nginx -y
```

## 启动

两条命令：

```bash
sudo systemctl start nginx
sudo systemctl enable nginx  # 设置开机自启动
```

查看 nginx 运行状态：

```bash
sudo systemctl status nginx
```

## 配置

```nginx
# 定义一个虚拟主机，用来响应 http 请求。
server {
    listen 80;  # 监听 80 端口
    server_name _;  # 匹配所有的域名
    ...
}

location /order-badminton {
    root /var/www/html;
    index index.html;
}
```

* location：当访问 `http://ip/order-badminton` 时，会从 /var/www/html/order-badminton 目录下寻找资源。
* root：拼接规则是请求路径 /order-badminton/... 会跟在 root 路径后形成 `http://ip/order-badmint\example.png`

### root

 root 拼接规则：请求路径会拼在 root 路径后面

举个例子：

* 请求路径：http://ip/order-badminton/xxx.html
* nginx 处理：`/var/www/html` +  `/order-badminton/xxx.html` = `/var/www/html/order-badminton/xxx.html`



