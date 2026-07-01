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
  <sub>手机扫码体验</sub>
</p>

## 这是什么

微信扫码，和一个随机的陌生人聊一小时。

## 能干什么

- 扫码进入微信，发 `打开`，系统自动匹配一个同时在线的人
- 两人 1v1 对话，限时 60 分钟，到点自动断开
- 发 `暂停` 停止匹配，发 `打开` 重新开始
- 发 `离开` 提前结束当前对话
- 举报对方，被举报三次的人自动封禁
- 不注册、不留名、不存聊天记录

## 技术

Next.js + PostgreSQL + Prisma + 微信 OpenClaw

## 跑起来

```bash
npm install
cp .env.example .env
DATABASE_URL='postgresql://yuriwong@localhost:5432/whoareyou' npx prisma migrate deploy
DATABASE_URL='postgresql://yuriwong@localhost:5432/whoareyou' npx prisma generate
DATABASE_URL='postgresql://yuriwong@localhost:5432/whoareyou' ADMIN_TOKEN='dev-admin-token' PROVIDER_USER_HASH_SECRET='dev-provider-user-hash-secret' PROVIDER_CREDENTIAL_ENCRYPTION_SECRET='whoareyou-dev-provider-credential-encryption-secret' npm run dev
```

Web: http://localhost:3000 · Admin: http://localhost:3000/admin

## 部署

See [docs/deploy.md](docs/deploy.md).

```text
web                npm run start
openclaw-updates   npm run worker:openclaw-updates:loop
outbox-worker      npm run worker:outbox:loop
scheduled-worker   npm run worker:scheduled:loop
release/migration  npm run db:migrate
```

## Contact

`samwang0041@gmail.com`
