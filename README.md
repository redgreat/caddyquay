# caddy-quay-sync

通过 GitHub Actions 每天定时将 Docker Hub 上的最新 [caddy](https://hub.docker.com/_/caddy) 镜像同步推送到 Quay.io 仓库，方便在国内/无法直接访问 Docker Hub 的环境下从 Quay 拉取镜像。

## 仓库

- 目标仓库：`quay.io/zrcrm/caddy`

## 工作原理

工作流定义在 [`.github/workflows/sync-caddy.yml`](.github/workflows/sync-caddy.yml)：

1. 使用仓库 Secrets 中的凭证登录 quay.io
2. `docker pull caddy:latest` 拉取 Docker Hub 最新镜像
3. 重新打标签为 `quay.io/zrcrm/caddy:latest`
4. 推送到 Quay.io

## 配置

在 GitHub 仓库中进入 **Settings → Secrets and variables → Actions → New repository secret**，添加以下两个 Secret：

| Secret | 说明 |
| ------ | ---- |
| `QUAY_USERNAME` | Quay.io 用户名（或 Robot Account 名称） |
| `QUAY_PASSWORD` | Quay.io 密码（或 Robot Account 的 Token） |

## 使用

推送镜像到 Quay：

```bash
docker pull quay.io/zrcrm/caddy:latest
docker run -d -p 80:80 -p 443:443 quay.io/zrcrm/caddy:latest
```

## 触发方式

- 每天 UTC 00:00 自动执行
- 手动触发：仓库 **Actions** 页面 → 左侧选择 `Sync Caddy Image to Quay` → 点击 **Run workflow**
