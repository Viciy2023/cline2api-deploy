# cline2api-deploy

自动同步上游 [luawei1/cline2api](https://github.com/luawei1/cline2api) 并构建 Docker 镜像。

## 镜像

- Docker Hub: `ccy2026/cline2api:latest`
- GHCR: `ghcr.io/viciy2023/cline2api-deploy:latest`

## 机制

GitHub Actions 每 4 小时检查上游 `main` 分支 SHA，与 `last-build.txt` 指纹比对；有变化则克隆上游源码，用上游自带的 `Dockerfile` 构建镜像并推送到 GHCR 和 Docker Hub，随后回写指纹。也可在 Actions 页手动触发（workflow_dispatch）。

## FNOS 部署

```yaml
services:
  cline-proxy:
    image: ccy2026/cline2api:latest
    container_name: cline-proxy
    restart: unless-stopped
    ports:
      - "3457:3457"
    volumes:
      - ./.cline-accounts.json:/app/.cline-accounts.json
      - ./override.md:/app/override.md:ro
    environment:
      - PORT=3457
      - CLINE_PROXY_HOST=0.0.0.0
```
