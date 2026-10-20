# Ayakaleaf Pro - LazyCat App

[Ayakaleaf Pro](https://github.com/ayaka-notes/ayakaleaf-pro)（Overleaf Community Edition 增强版：Git 同步、修订追踪、Zotero/Mendeley、AI 助手、管理面板等）的懒猫微服（LazyCat）打包。

## 模式

- **镜像模式（mirror）**：`sharelatex/sharelatex` 镜像经 `docker.1ms.run` 加速器拉取，`require_digest_match: true` 校验镜像内容与上游一致。
- **自动跟随上游**：每日检查上游 semver tag，发现新版本自动构建发布。
- **喵喵商店发布**：仅私有商店（MiaoMiao private store），不发布官方商店。

## 架构（与上游 compose 一致）

| 服务 | 说明 |
| --- | --- |
| sharelatex 6.3.1 | 主服务（web + 全部微服务一体，端口 80） |
| mongo:8.0 | MongoDB（replSet 必须，自动初始化副本集） |
| redis:6.2 | Redis |

## 说明

- **数据卷**：`/lzcapp/var/ayakaleaf/sharelatex`（项目/编译/用户数据）、`mongo/`、`redis/`
- **首位注册用户自动成为管理员**
- `OVERLEAF_SITE_URL` 自动指向懒猫子域名（邀请链接、邮件功能依赖）
- `OVERLEAF_INVITE_TOKEN_SECRET` 稳定生成
- ⚠️ **沙箱编译（Sandboxed Compiles）已关闭**：该功能需要宿主 docker.sock（sibling containers），懒猫容器环境不挂载；常规编译（内置 TeX Live）不受影响
- 健康检查 `/health_check`（启动较慢，start_period 120s）

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `APP_ID` | 可选 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |

## 许可

AGPL-3.0（上游）
