# dump — Katabump Auto Renew

自动续期 Katabump 服务器的自动化脚本。利用 Playwright 和 CDP 技术模拟用户操作，可绕过 Cloudflare Turnstile 验证码。

## 配置 GitHub Actions  Secrets

在仓库的 **Settings → Secrets and variables → Actions** 中添加以下变量：

| Secret 名称 | 是否必填 | 说明 |
|---|---|---|
| `USERS_JSON` | 是 | 账号密码，JSON 格式 |
| `HTTP_PROXY` | 否 | 代理地址，格式 `http://user:pass@host:port` |
| `TG_BOT_TOKEN` | 否 | Telegram Bot Token，用于发送通知 |
| `TG_CHAT_ID` | 否 | Telegram Chat ID |

### USERS_JSON 格式

```json
[{"username": "your_email@example.com", "password": "your_password"}]
```

支持三种格式：

1. **数组**（多账号）：
   ```json
   [{"username": "user1@example.com", "password": "pass1"}, {"username": "user2@example.com", "password": "pass2"}]
   ```

2. **带 `users` 键的对象**：
   ```json
   {"users": [{"username": "user1@example.com", "password": "pass1"}]}
   ```

3. **单个对象**：
   ```json
   {"username": "user1@example.com", "password": "pass1"}
   ```

### 在线生成 JSON 工具

将账号密码填入后生成 JSON：

```bash
python3 -c "import json; print(json.dumps([{'username': 'your_email@example.com', 'password': 'your_password'}]))"
```

## 本地运行

```bash
npm install
node action_renew.js
```

需要环境变量：

```bash
export USERS_JSON='[{"username":"xxx","password":"xxx"}]'
export HTTP_PROXY="http://user:pass@host:port"   # 可选
export TG_BOT_TOKEN="xxx"                        # 可选
export TG_CHAT_ID="xxx"                          # 可选
```

## Schedule

默认每天北京时间下午 6:05 自动执行，可在 `.github/workflows/renew.yml` 中修改 cron 表达式。