# workbuddy-demo

WorkBuddy × GitHub 连通性验证仓库，用于验证连接器的读写、提交与仓库管理能力。

## 当前状态

- 远程仓库：`leozhangxiaoyun123/workbuddy-demo`（Public）
- 本地仓库：`workbuddy-demo/`，分支 `main`，`origin` 已指向上述地址
- 推送通道：WorkBuddy GitHub 连接器（服务端调用 `api.github.com`）；写接口当前返回 403，待重新授权连接器或改用 PAT 后重试

## 背景：为什么仓库要在网页侧创建

GitHub 连接器使用 GitHub App 安装令牌，`POST /user/repos`（创建个人仓库）与 `/repos/{owner}/{repo}/forks` 均返回 `403 Resource not accessible by integration`。GitHub 官方将该端点标记为 `enabledForApps: false`，安装令牌没有用户身份，加权限或重新授权都无法绕过。因此个人仓库需要在 GitHub 网页侧创建一次，之后的读写、PR、Issue 等操作均可由连接器完成。

## 关于本机 git 通道

当前环境的本地代理只对 `api.github.com` 放行，`github.com` 返回 `CONNECT tunnel failed, response 502`；绕过代理直连时，网页请求约 12 秒超时、git smart-HTTP 完全失败。因此本机 `git push` / `git clone` 暂不可用，配置 PAT 也无法绕过这个网络层问题。连接器的服务端调用不受影响。

待网络条件改善后可用：

```bash
git push -u origin main
git clone https://github.com/leozhangxiaoyun123/workbuddy-demo.git
```

HTTPS 推送需要 Personal Access Token（GitHub 已不支持账号密码）。
