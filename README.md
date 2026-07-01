# UNKNOWN

<p align="center">
  <img src="public/whoareyou-main-visual-abstract.png" alt="UNKNOWN main visual" width="100%" />
</p>

<p align="center">
  <a href="https://chat.wangyuzhao.cn/">体验 UNKNOWN</a> ·
  <a href="mailto:samwang0041@gmail.com">Contact</a>
</p>

<p align="center">
  <a href="https://chat.wangyuzhao.cn/">
    <img src="public/unknown-experience-qr.png" alt="UNKNOWN experience QR code" width="180" />
  </a>
</p>

<p align="center">
  <sub>手机扫码体验 · Scan to try</sub>
</p>

## What is UNKNOWN

UNKNOWN 是一个基于微信的 1v1 随机匹配对话产品。用户扫码进入微信，发送「打开」，系统会把你和另一个同时在线的陌生人配对，限时一小时对话。

核心特点：

- **扫码即入**：通过微信 OpenClaw 入口，无需注册、无需下载 App。
- **1v1 限时**：每段对话一小时，每 10 分钟一次产品声音提醒。
- **完全匿名**：不保存昵称、头像、手机号或明文聊天记录。
- **不沉迷**：没有信息流，没有无限续杯，没有让你停不下来的机制。
- **开源**：代码完全开源，隐私优先。

体验地址：[chat.wangyuzhao.cn](https://chat.wangyuzhao.cn/)

## How It Works

1. 用户扫描网页上的二维码，进入微信 OpenClaw 入口。
2. 发送 `打开` 开启随机匹配。
3. 如果此时有另一个人也在线，两人被配对进入一小时对话。
4. 任一方发送 `离开` 可提前结束；`暂停` 关闭匹配，`打开` 重新开启。
5. 断开或离开后进入短暂冷却，不会再次匹配到同一个人。

## Tech Stack

- **Frontend**: Next.js App Router
- **Database**: PostgreSQL + Prisma
- **Provider**: WeChat OpenClaw (微信 iLink)
- **Testing**: Vitest + Playwright E2E

## 当前 MVP

- Next.js web entry and private admin dashboard.
- PostgreSQL + Prisma for users, connections, reports, pair blocks, outbox, jobs, errors, and metrics.
- QR session route at `/api/qr`, returning a generated QR image, session id, status URL, and expiry.
- One-hour matching, 10-minute reminders, leave/report/pair-block flow.
- 24-hour provider reachability window with renewal prompt and expiry.
- Privacy-safe admin overview, health, safety, and anonymous connection pages.
- Outbox and scheduled-job workers for database-backed retries and timed behavior.
- Unit, integration, and Playwright E2E coverage.

## 微信二维码接入

The landing page follows a provider-neutral QR contract:

```text
GET /api/qr -> imageSrc, sessionId, statusUrl, expiresAt
GET statusUrl -> waiting_to_scan | scan_confirming | confirmed | verification_required | expired | provider_error
```

Local fake mode:

```bash
PROVIDER_MODE=fake npm run dev
```

Real Weixin QR mode:

```bash
PROVIDER_MODE=openclaw npm run dev
```

In real mode, `/api/qr` calls Weixin iLink QR endpoints to generate a scannable QR code. The outbox worker sends messages via `ilink/bot/sendmessage`.

## Privacy Boundaries

- User 只存储 keyed HMAC-SHA-256 hash，不保存任何个人信息。
- Provider credentials 使用 AES-GCM 加密存储。
- No nickname, avatar, phone number, or profile field is collected.
- Admin APIs 不返回原始 provider refs 或明文数据。

## Local Development

```bash
npm install
cp .env.example .env
DATABASE_URL='postgresql://yuriwong@localhost:5432/whoareyou' npx prisma migrate deploy
DATABASE_URL='postgresql://yuriwong@localhost:5432/whoareyou' npx prisma generate
DATABASE_URL='postgresql://yuriwong@localhost:5432/whoareyou' ADMIN_TOKEN='dev-admin-token' PROVIDER_USER_HASH_SECRET='dev-provider-user-hash-secret' PROVIDER_CREDENTIAL_ENCRYPTION_SECRET='whoareyou-dev-provider-credential-encryption-secret' npm run dev
```

- Web: `http://localhost:3000`
- Admin: `http://localhost:3000/admin`

## Production Deploy

See [docs/deploy.md](docs/deploy.md).

```text
web                npm run start
openclaw-updates   npm run worker:openclaw-updates:loop
outbox-worker      npm run worker:outbox:loop
scheduled-worker   npm run worker:scheduled:loop
release/migration  npm run db:migrate
```

## 不是

- 不是匿名交友软件。
- 不是公开大流量陌生人聊天室。
- 不是另一个让人停不下来的内容机器。
- 不是用 AI 冒充人，也不是用人冒充 AI。

## Contact

`samwang0041@gmail.com`
