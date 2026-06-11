# ModelScope Lab

AI 模型接口性能检测工具。前端负责跑测 OpenAI-compatible 接口，Node 后端只负责托管页面。

## 本地运行

```bash
export PORT=8088
npm start
```

打开 `http://localhost:8088/`。

## 部署要点

- `PORT`：服务监听端口。默认 `8088`，可避开服务器上已占用的端口。

## 上传到 GitHub

先在 GitHub 创建一个空仓库，然后在本地执行：

```bash
git add .
git commit -m "Initial ModelScope Lab"
git branch -M main
git remote add origin git@github.com:YOUR_NAME/YOUR_REPO.git
git push -u origin main
```

如果使用 HTTPS，把最后两行换成 GitHub 给你的 HTTPS 地址即可。

## 服务器部署

示例把项目放到 `/opt/modelscope-lab`，服务端口用 `8088`：

```bash
sudo mkdir -p /opt/modelscope-lab
sudo chown -R "$USER":"$USER" /opt/modelscope-lab
git clone git@github.com:YOUR_NAME/YOUR_REPO.git /opt/modelscope-lab
cd /opt/modelscope-lab
cp .env.example .env
nano .env
npm run check
sudo cp deploy/modelscope-lab.service /etc/systemd/system/modelscope-lab.service
sudo systemctl daemon-reload
sudo systemctl enable --now modelscope-lab
sudo systemctl status modelscope-lab
```

测试配置只保存在浏览器本机 `localStorage`，不会保存到服务器。

## Nginx 反代示例

```nginx
server {
    server_name mail4.space;

    location / {
        proxy_pass http://127.0.0.1:8088;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

