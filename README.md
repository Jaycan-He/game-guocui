# 炸金花多人网页版（云部署包）

这是一个手机/电脑浏览器可用的 Node.js + WebSocket 多人游戏原型，房间最多 8 人。

## 云端部署（Render 免费 Web Service）

1. 解压本 ZIP，把里面的 `public` 文件夹、`server.js`、`package.json`、`test.js`、`README.md` 放进同一个 GitHub 仓库的根目录（不要把外层文件夹再套一层）。
2. 登录 GitHub，创建一个新仓库，例如 `zhajinhua-multiplayer`，将以上文件上传并提交。
3. 打开 Render： https://render.com/ ，用 GitHub 登录。
4. 在 Render Dashboard 选择 **New → Web Service**，连接刚创建的 GitHub 仓库。
5. 配置：
   - Runtime：Node
   - Build Command：`npm install`
   - Start Command：`npm start`
   - Instance Type：Free（如果账号当前提供该选项）
6. 点击 **Deploy Web Service**，等待构建完成。Render 会分配一个 `https://xxxx.onrender.com` 网址。
7. 用这个 HTTPS 网址打开游戏，创建房间后把房间号和网址发给朋友。网页会自动使用安全 WebSocket（WSS）连接服务器，不需要单独配置端口。

官方说明：
- Render Web Service 部署： https://render.com/docs/web-services
- Render WebSocket 支持： https://render.com/docs/websocket
- Render 免费实例限制： https://render.com/docs/free

## 免费版注意事项
- 免费服务连续 15 分钟没有流量后会休眠；下次访问通常需要约一分钟唤醒。
- 免费服务可能重启，房间和进行中的牌局属于内存状态，重启后会丢失。
- 这是朋友之间试玩的开发原型，不是经过安全审计的正式游戏服务。不要用真钱或存放有价值的积分。

## 本地运行和测试
需要 Node.js 18 或更高版本：

```bash
npm install
npm test
npm start
```

本地打开 `http://localhost:8080`。

## 主要规则
- 每位玩家入房 300 分；每局底注 5 分。
- 闷牌 5/10/15；看牌下注为对应闷牌金额两倍。
- 最多 8 人；服务器管理手牌、积分和行动顺序。
- 第一局随机庄家，之后由上一局赢家做庄；按座位循环行动。
- 235 可打败所有豹子（包括 AAA），但顺金大于 235；AAA 是普通豹子最大牌。
- 闷牌拿到豹子/顺金并获胜，或闷牌 235 使其他人全部弃牌，可从每位其他玩家获得额外 30 分（不足则扣除剩余分数）。
- 房主可开始新局和终止进行中的牌局；终止时退还本局已投入分数。

## 项目文件
- `public/index.html`：网页界面
- `server.js`：HTTP + WebSocket 服务器
- `package.json`：启动与依赖配置
- `test.js`：规则测试
