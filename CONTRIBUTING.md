# 贡献指南

最有价值的三类贡献（全部无需包含任何会话内容）：

1. **客户端适配**：你环境里某客户端的数据目录路径 + 一条记录的字段结构（键名即可，值脱敏）。对应改 `scripts/collect_stats.py` 的 `CLIENTS` 注册表，并更新 `references/VALIDATION.md` 状态格。
2. **分类校准**：被误分类的任务话术模式（改述即可，不要原文）→ `TASK_RULES` 加词。
3. **行业覆盖层**：你所在行业的"职业事故清单"（AI 用错会出什么事）+ 工具基准（专业级/够用档）。这是行业版评分规则的原料，见 `references/rubric.md` 的行业覆盖层格式。

隐私红线：任何提交不得包含真实会话内容、姓名、公司可识别信息。

---

## Contributing (English)

The most valuable contributions — none of them require sharing session content:

1. **Client adapters**: the data directory of an AI client on your machine plus the field structure of one record (key names only, values redacted). Update the `CLIENTS` registry in `scripts/collect_stats.py` and the status cell in `references/VALIDATION.md`.
2. **Classification calibration**: paraphrased task patterns that get misclassified → add keywords to `TASK_RULES`.
3. **Industry overlays**: a list of "professional incidents" (what goes wrong when AI is misused) and tool baselines for your industry. Format: see the industry overlay section in `references/rubric.md`.

### Workflow
- Open an issue first for anything non-trivial; small fixes can go straight to a PR.
- CI (`.github/workflows/ci.yml`) compiles the scripts, validates `SKILL.md` frontmatter, smoke-tests the collector and checks relative Markdown links. Run `python -m py_compile scripts/*.py` locally before pushing.
- Add a line to `CHANGELOG.md` for user-facing changes.

**Privacy red line:** no real session content, names, or company-identifiable information in any commit, issue, or PR.
