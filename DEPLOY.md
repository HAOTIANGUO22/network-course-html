# 部署说明

## 1. 上传文件

把本文件夹上传到服务器，例如：

```bash
/www/network-course-html
```

## 2. 启动服务

进入目录：

```bash
cd /www/network-course-html
npm install
npm start
```

默认端口是 `5000`。访问：

```text
http://服务器IP:5000/
```

如需改端口：

```bash
PORT=5000 npm start
```

## 3. 后台常驻（推荐 pm2）

```bash
npm install -g pm2
cd /www/network-course-html
npm install
pm2 start server.js --name network-course-html
pm2 save
pm2 startup
```

查看状态：

```bash
pm2 status
pm2 logs network-course-html
```

## 4. Nginx 反向代理示例

如果你有域名，例如 `course.example.com`：

```nginx
server {
    listen 80;
    server_name course.example.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

改完后：

```bash
nginx -t
systemctl reload nginx
```

