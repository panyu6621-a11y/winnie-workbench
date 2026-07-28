# Winnie 工作台 - 部署指南

这是一个完整的 **PWA 工作台** 项目，包含：
- 前端 `index.html`：莫兰迪色系 PWA，可添加到手机桌面
- 后端 `server.py`：Flask 服务，提供真实每日资讯（RSS 抓取）+ 英语/韩语学习内容

## 文件说明

```
pooh-workbench/
├── index.html          # 前端 PWA（所有图标已内联）
├── server.py           # Flask 后端
├── requirements.txt    # Python 依赖
├── generate.py         # 生成 index.html 的脚本（可忽略）
├── start.sh            # 本地启动脚本
└── icons/              # 维尼熊图标源文件（可选）
```

## 本地运行

```bash
cd pooh-workbench
pip install -r requirements.txt
python3 server.py
```

访问 http://localhost:18926

## 长期部署（推荐 Render）

### 1. 创建 Render 账号
前往 https://render.com 用 GitHub 登录。

### 2. 上传代码到 GitHub
把 `pooh-workbench` 目录推送到一个 GitHub 仓库。

### 3. 在 Render 创建 Web Service
- 点击 **New → Web Service**
- 选择刚才的 GitHub 仓库
- 配置：
  - **Runtime**: Python 3
  - **Build Command**: `pip install -r requirements.txt`
  - **Start Command**: `gunicorn -b 0.0.0.0:$PORT server:app`
  - **Plan**: Free
- 点击创建，等待部署完成

### 4. 访问链接
Render 会给你一个类似 `https://winnie-workbench.onrender.com` 的链接，这就是你的长期工作台链接。

## 添加到手机桌面

1. 用手机浏览器打开 Render 链接
2. iPhone：Safari 底部分享按钮 →「添加到主屏幕」
3. Android：Chrome 菜单 →「添加到主屏幕」
4. 桌面上会出现维尼熊图标的 App，点开全屏使用

## 可选：AI 增强英语/韩语

如果你希望英语/韩语每天由 AI 生成不同内容：

1. 在 Render 的 **Environment** 中添加环境变量：
   - Key: `OPENAI_API_KEY`
   - Value: 你的 OpenAI API Key
2. 重启服务

> 不设置 AI Key 时，英语/韩语会使用内置的 30 天高质量内容库循环。

## 注意事项

- 资讯来自 RSS（少数派、36氪），每 1 小时后端会自动刷新一次
- 如果 RSS 抓取失败，会自动使用内置资讯兜底
- 所有个人数据（待办、体重、生理期、打卡）都保存在浏览器 localStorage 中
- 首次添加到桌面后，建议打开一次并停留几秒，让 Service Worker 完成缓存
