# ui组业务组件 skill 安装

## 内容
- `arco-admin-ui/` — Codex skill 本体（目录名与内部 `name` 保持不变，确保可调用）

## 安装到本机 Codex
1. 解压本包
2. 将 `arco-admin-ui` 文件夹复制到：
   - Windows: `%USERPROFILE%\.codex\skills\arco-admin-ui`
3. 新开 Codex 对话后使用：
   - `$arco-admin-ui 做一个用户管理列表页`
   - 或直接说「按 ui组业务组件规范做中后台」

## 校验（可选）
```bash
python %USERPROFILE%\.codex\skills\.system\skill-creator\scripts\quick_validate.py %USERPROFILE%\.codex\skills\arco-admin-ui
```

## 版本
- name: arco-admin-ui
- 展示名: ui组业务组件
- 打包日期: 2026-07-19
