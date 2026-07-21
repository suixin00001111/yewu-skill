# ui组业务组件 Skill 说明文档

| 项 | 内容 |
|----|------|
| 显示名 | **ui组业务组件** |
| 调用 | `$arco-admin-ui`（内部 skill 目录名，仅工具标识） |
| 安装路径 | `%USERPROFILE%\.codex\skills\arco-admin-ui` |
| 定位 | 中后台业务系统 UI / 业务组件规范与代码生成 |

> 本文档与生成页面中 **不出现** 第三方设计系统或厂商品牌名。  
> 组件 API 与默认视觉以 **实现层中后台组件库** 为准；产品壳与业务版式以本 skill 为准。

---

## 1. 这个 skill 是什么

用于 **中后台 / Admin / 工作台 / 列表 / 详情** 的界面生成、改写与评审。

- **组件怎么写、长什么样**：跟实现层组件库（Button、Table、Form、Progress、Message、Modal…）的标准用法走。  
- **壳与业务版式**：按本 skill 锁定的 ui组样式（顶栏、侧栏、列表标题、弹窗顶栏渐变等）。  
- **界面与对外说明**：只称 **ui组业务组件**，不写外部品牌。

---

## 2. 双层规则（避免冲突）

| 层级 | 内容 | 权威 |
|------|------|------|
| A. 产品壳 & 业务结构 | 顶栏 `#004EA2`、侧栏选中 `#E6F1FF`、工作台无页头标题、列表标题单独一行、详情页头操作条、模块 16–20 边距 | **本 skill 强制** |
| B. 组件 API / 视觉 | size、status、Table 描边、Progress、Message、Form 校验 | **实现层组件库** |
| 覆盖特例 | 弹窗顶栏背景 | **本 skill**：`linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%)` |

详见 `references/component-source.md`。

---

## 3. 硬约束速查

### 3.1 壳

- 顶栏：通栏 `#004EA2` + 白字系统名（按业务填）+ 右侧工具（含明暗切换）
- 侧栏：白底；选中 `#E6F1FF` + 蓝色图标/字
- 主色按钮：`#165DFF`
- 内容区：浅灰底 + 白卡片；模块内边距 16–20px
- 图标：IconPark outline 优先

### 3.2 页面结构

| 页面 | 标题 | 操作 |
|------|------|------|
| 工作台 | **不要**内容区大标题 | 业务按钮在内容模块内 |
| 列表 | 简洁页名**单独一行** | 新建等在筛选下、表格上工具行右侧 |
| 详情/编辑 | 页头条（有背景）左标题右返回/取消/保存 | 页面级按钮只在顶条 |

### 3.3 弹窗 Modal

- 三类结构：纯确认 / 表单 / 确认+输入（见 `modal-patterns.md`）
- **文案不固定**：标题、说明、表单项、按钮文字均按用户/业务配置，文档示例仅作结构参考
- 顶栏背景（强制）：

```css
background: linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%);
```

- 底栏右对齐：取消在左，主色确认在右

### 3.4 全局提示 Message

- 删除/保存/提交的**结果**用 Message（success/error…）
- 删除前确认用 Modal/Popconfirm，**成功后必须**成功提示（如「删除成功」）
- 默认顶部居中，约 3s（见 `message-patterns.md`）

### 3.5 进度条 Progress

- 按实现层 Progress：当前进度 + 中性总轨道 + 可选百分比
- `line` / `circle`；状态 normal / success / warning / error
- 业务阶段用 **Steps**，不要用 Progress 冒充步骤（见 `progress-patterns.md`）

### 3.6 表格

- 浅灰表头；列表至少外边框；详情明细完整网格描边
- 操作列 text 按钮；使用实现层 Table API

---


## 3.1 完善后的生成链路（推荐）

1. **Page Spec**（`page-spec.md`）— 类型、模块、接口、反馈  
2. **套模板**（`templates/workbench|list|detail.md`）  
3. 填业务文案与 mock/services 占位（`async-states.md`）  
4. 按需 Modal / Message / Progress / Drawer / 批量（`ops-patterns`）  
5. **delivery-gates.md 全过** 再交付  

### 新增文件

| 文件 | 作用 |
|------|------|
| page-spec.md | 页面契约 |
| async-states.md | loading/empty/error/接口占位 |
| delivery-gates.md | 交付闸门 |
| templates/* | 三页最小骨架 |
| drawer-patterns.md | 抽屉 |
| ops-patterns.md | 批量/空态/权限/筛选/上传轻量 |


## 4. 文件结构

```
arco-admin-ui/                 # 工具目录名（调用 ID）
  SKILL.md                     # 入口
  README.md                    # 本说明
  agents/openai.yaml           # 显示名「ui组业务组件」
  references/
    component-source.md        # 实现层 vs 本 skill 覆盖
    product-shell.md
    style-usage.md
    design-tokens.md
    components.md
    component-visual.md
    layout-patterns.md
    content-ux.md
    modal-patterns.md
    message-patterns.md
    progress-patterns.md
    page-spec.md
    async-states.md
    delivery-gates.md
    drawer-patterns.md
    ops-patterns.md
    templates/
    theme-mode.md
    do-dont.md
    image-prompts.md
    examples/*.png
```

---

## 5. 使用方式

1. 在 Codex 中：`$arco-admin-ui`，或描述中后台/工作台/列表等。  
2. 说明业务名、页面类型、技术栈（默认 React）。  
3. 生成后自检：壳色、列表标题、Message、Progress、弹窗顶栏渐变、**无外部品牌文案**。  

**改 skill 后**：建议新开任务再调用。

---

## 6. 维护建议

- examples 截图若与规则冲突：**以文字硬约束为准**，并择机换图。
- `workbench-ops.png` 体积较大，打包时可压缩。


1. 安装目录与桌面源修改后保持同步。  
2. 新增组件规范：在 `references/` 增文档 → `SKILL.md` 挂载 → 更新本 README。  
3. 实现层大版本升级时：对照 Table/Form/Message/Progress API 再改 demo。  
4. 对外分享本 skill 时，说明文档与示例 UI **均使用「ui组业务组件」命名**。

---

## 7. 演示页（可选）

工作区示例：

`Documents\Codex\2026-07-20\c-users-15828-desktop-2-arco\outputs\ui-biz-3pages.html`

含工作台 / 列表 / 详情、明暗切换、三类弹窗、全局结果提示。
