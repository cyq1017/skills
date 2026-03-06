# xhs-note-manager

小红书笔记管理技能。支持通过笔记 ID 自动删除笔记或修改笔记为“仅自己可见”。

## 功能支持

1. **删除笔记** (`--action delete`)
2. **设为私密（仅自己可见）** (`--action private`)

系统使用双通道机制保证稳定性：
- **API 通道**：尝试通过官方/非公开接口调用，速度极快（目前 xhs 库对部分管理功能可能受限）。
- **浏览器自动化通道 (Playwright)**：作为兜底方案，模拟用户登录小红书创作中心，自动查找指定笔记并执行删除或设为私密操作。

## 使用条件与环境

1. 需要 Python 3 环境。
2. 安装依赖：`pip install -r requirements.txt`，如果使用自动化通道则还需执行 `playwright install`。
3. 在项目根目录的 `.env` 中提供必需的登录凭证：
   - `XHS_COOKIE`：包含有效登录信息的 Cookie。
   - （自动化模式下也可能直接利用现有打开的浏览器或者输入 Cookie 进行自动登录）。

## 快速使用

### 删除笔记

```bash
python skills/xhs-note-manager/scripts/manage_xhs.py --action delete --note-id [笔记ID]
```

### 设为私密（仅自己可见）

```bash
python skills/xhs-note-manager/scripts/manage_xhs.py --action private --note-id [笔记ID]
```

## 注意事项

- **ID 查找**：笔记 ID（如 `69aa0c50000000002203ad26`）可以从小红书的笔记详情页链接中获取。
- **环境安全**：操作涉及账号权限，请妥善保管 `.env` 中的 `XHS_COOKIE`。
- 自动化通道在无头模式（headless）下运行，如需调试可调整代码中的 playwright 设置。
