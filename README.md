# flow2api-skill-mcp

通过本机 [Flow2API](https://github.com/JYAIXR/flowapi)（MIT，© TheSmallHanCat）服务生图的 Agent Skill 与 MCP 服务。本仓库不包含 Flow2API 源码，只通过其 HTTP API 调用，使用前需自行部署 Flow2API。

- `skill/flow2api-new-skill/SKILL.md`：给 Claude Code / Codex / Antigravity 用的 skill，说明何时、如何调用生图工具。
- `mcp/`：Python MCP 服务（`server.py`），提供单图生成、多账号批量生成、模型/账号列表、连接检查等工具，也可作命令行使用。详见 [mcp/README.md](mcp/README.md)。

## 安装

```bash
cd mcp
python3 -m venv .venv
.venv/bin/pip install -r requirements.lock
```

在 `mcp/` 下创建私密配置 `settings.json`（已被 .gitignore 忽略，切勿提交）：

```json
{
  "base_url": "http://127.0.0.1:38000/v1",
  "api_key": "<你的 Flow2API API Key>",
  "output_directory": "/absolute/path/for/images",
  "job_timeout_seconds": 600
}
```

可选字段：`media_proxy`（下载远程图片用的代理）、`batch_max_jobs`、`batch_max_concurrency`、`batch_default_concurrency`。

注册 MCP（以 Claude Code 为例）：

```bash
claude mcp add flow2api -- /path/to/flow2api-mcp/.venv/bin/python /path/to/flow2api-mcp/server.py
```

把 `skill/flow2api-new-skill` 复制到 `~/.claude/skills/`（或对应客户端的 skills 目录），并把 SKILL.md 中的 `/path/to/flow2api-mcp` 替换为实际路径。

## 测试

```bash
cd mcp && .venv/bin/python -m unittest
```

## 许可证

[MIT](LICENSE)
