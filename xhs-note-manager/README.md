# xhs-note-manager

管理已发布小红书笔记的小技能。

## 功能

- 删除笔记（按 `note_id`）
- 设为仅自己可见（按 `note_id`）
- 支持执行模式：`auto`（API 优先 + 浏览器兜底）、`api`、`browser`

## 文件结构

```text
xhs-note-manager/
├── SKILL.md
└── scripts/
    └── manage_xhs.py
```

## 前置条件

1. Python 3.10+
2. 在 `.env` 配置 Cookie：

```env
XHS_COOKIE=你的完整cookie
```

3. 安装依赖（建议虚拟环境）：

```bash
pip install xhs python-dotenv playwright
playwright install chromium
```

## 使用方法

### 删除笔记

```bash
python xhs-note-manager/scripts/manage_xhs.py \
  --action delete \
  --note-id <note_id> \
  --mode auto
```

### 设为仅自己可见

```bash
python xhs-note-manager/scripts/manage_xhs.py \
  --action private \
  --note-id <note_id> \
  --mode auto
```

### 仅 API 模式

```bash
python xhs-note-manager/scripts/manage_xhs.py --action delete --note-id <note_id> --mode api
```

### 仅浏览器模式

```bash
python xhs-note-manager/scripts/manage_xhs.py --action delete --note-id <note_id> --mode browser
```

## 参数说明

- `--action`：`delete` 或 `private`
- `--note-id`：小红书笔记 ID
- `--mode`：`auto` / `api` / `browser`
- `--headful`：浏览器模式使用有头界面（便于调试）
- `--dry-run`：仅检查配置，不实际执行

## 注意事项

- 小红书接口可能变更，`private` 的 API 成功率不如 `delete` 稳定。
- 页面改版可能导致浏览器兜底选择器失效，需要按实际文案微调。
- 本工具仅用于你自己账号下笔记管理，请合法合规使用。
