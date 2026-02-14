# docker-ntfy

ntfy セルフホスト。

## 起動

```bash
docker compose up -d
```

Tailscale マシンも作成する場合は `TS_AUTHKEY` を <https://login.tailscale.com/admin/settings/keys> にて発行する。

- **Reusable: オン** — コンテナを再作成しても同じキーで再接続できる
- **Ephemeral: オン** — コンテナを落とすと Tailscale のマシン一覧から自動削除される
- **有効期限: 短め (1日など)** — 登録完了後は不要になるため

```bash
# .env に TS_AUTHKEY を設定する。
docker compose --profile tailscale up -d
```

## ユーザー作成

認証がデフォルトで有効なため、初回起動後にユーザを作成する。

```bash
# ユーザ作成。
docker compose exec ntfy ntfy user add --role=admin youruser

# トークン発行。
docker compose exec ntfy ntfy token add youruser
```

## 通知テスト

```bash
curl -H "Authorization: Bearer tk_xxxxxxxx" -d "テスト" http://localhost:2586/mytopic
```
