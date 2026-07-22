---
name: arco-admin-ui
description: >
  ui组业务组件：中后台 UI/设计系统与业务组件规范。用于生成、改写、评审 Web 管理后台界面、
  业务组件代码、以及符合本规范视觉的 UI 示意图/mockup。在以下情况必须使用：
  (1) 中后台、Admin、Dashboard、控制台、运营后台、B 端管理系统；
  (2) 用户提到 ui组业务组件、业务组件库、中后台组件规范、工作台、组织列表；
  (3) 需要 Layout/Menu/Table/Form/Modal 等企业后台组件；
  (4) 生成符合本规范的界面代码或 UI 图；
  (5) 检查现有后台是否符合本规范风格。
  实现层默认 React 中后台组件库；若用户指定 Vue 则用对应 Vue 实现层。包名仅用于代码依赖，界面与说明不出现外部品牌。
---

# ui组业务组件

按 **ui组业务组件** 规范产出中后台界面与 UI 图。产品壳以参考图为准：
**深蓝通栏顶栏 + 浅色侧栏浅蓝选中菜单**。禁止套用其它后台默认壳、
通用 AI 紫渐变/玻璃拟态等非本规范审美。

本 skill 始终为 **产品态（中后台）**：优先信息密度、可扫描、业务字段。
样式与组件用法见 [references/style-usage.md](references/style-usage.md)。

## 硬约束

- **顶栏必须**：通栏深蓝 #004EA2，白字系统名（按业务填写，勿固定示例名）+ 右侧工具区（见 [references/product-shell.md](references/product-shell.md)）
- **菜单必须**：白底侧栏；选中 `#E6F1FF` + 主色图标/字；**默认可折叠**：展开 220 / 收起 **48 仅图标**；`Menu.collapse` 同步 `Sider.collapsed`；**禁止**侧栏与内容间灰滚动轨、双 trigger、默认 resize（强制读 [references/sider-collapse.md](references/sider-collapse.md)）
- 主色按钮/链接默认 #165DFF（primary-6）；语义色/中性色见 [references/design-tokens.md](references/design-tokens.md)
- **样式用法**（字号 14、四级灰、4 倍间距、圆角≤8、反馈层级、按钮唯一主 CTA）见 [references/style-usage.md](references/style-usage.md)
- 组件 API 与 size 阶梯见 [references/components.md](references/components.md)；**单组件视觉（含 Avatar 等）**见 [references/component-visual.md](references/component-visual.md)
- 布局默认：顶栏深蓝 + 侧栏白 + 内容浅灰底白卡片，见 [references/layout-patterns.md](references/layout-patterns.md)
- 工作台/列表/详情气质参考 examples（壳层不可变）
- **工作台内容区**见 [references/content-ux.md](references/content-ux.md)（只优化内容区，不改壳）
- 出图前套用 [references/image-prompts.md](references/image-prompts.md)
- 禁止项见 [references/do-dont.md](references/do-dont.md)
- **页面级操作**（保存/返回/提交/取消）固定内容区最上方页头
- **模块分块**：每业务模块单独一块 Card
- **模块内边距**：内容区与 Card 内容统一 16–20px；Descriptions/分块表不得贴边
- **步骤条**：节点+连接线；**充满容器宽度**并保留模块内边距；界面不出现第三方库品牌名
- **表格**：浅灰表头；详情明细须 `border`+`borderCell`；列表主表至少 `border`
- **页面标题**：列表简洁页名且**单独一行**（不与新建等按钮并列）；**工作台不要**页头标题；详情页头条左标题右操作（有背景）
- **明暗切换**：实现层暗色 + CSS 变量；入口顶栏右侧；顶栏两态仍 #004EA2（见 theme-mode.md）
- 代码优先真实组件库组件
- **进度条 Progress**：线形/环形/状态按实现层 Progress；轨道中性、当前主色；与 Steps 区分，见 [references/progress-patterns.md](references/progress-patterns.md)
- **页面契约**：生成前写 Page Spec（类型/模块/接口占位/反馈），见 [references/page-spec.md](references/page-spec.md)
- **异步与接口**：loading/empty/error；写操作 Message；services 占位，见 [references/async-states.md](references/async-states.md)
- **交付闸门**：交付前过 [references/delivery-gates.md](references/delivery-gates.md)，任一项否即重做
- **组件实现**：单组件 API/视觉以**实现层组件库**为准（见 [references/component-source.md](references/component-source.md)）；壳与弹窗顶栏等以本 skill 硬约束覆盖
- **批量/空态/权限轻量**：列表批量在工具行左；Empty 可行动；见 [references/ops-patterns.md](references/ops-patterns.md)
- **抽屉 Drawer**：长表/侧滑详情，见 [references/drawer-patterns.md](references/drawer-patterns.md)
- **全局提示 Message**：操作结果轻反馈（删除成功/保存成功/失败等）顶部居中；类型 success/error/warning/info/loading；默认约 3s 自动关闭；删除链路=确认后 Message，见 [references/message-patterns.md](references/message-patterns.md)
- **弹窗 Modal**：顶栏 `linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%)` + 标题+关闭；底栏右对齐「取消 + 主色确认」；分确认/表单/确认+输入三类，文案（标题/正文/按钮）按用户与业务配置、示例非固定文案；见 [references/modal-patterns.md](references/modal-patterns.md)
- **图标**：IconPark SVG（outline 优先）；禁止 Emoji 图标体系

## 技术栈默认（实现层包名勿改；勿写入界面文案）

| 项 | 默认 |
|----|------|
| React | 实现层 React 组件库 + 全局 CSS |
| Vue | 实现层 Vue 组件库（仅用户指定时） |
| 图标 | IconPark（@icon-park/react / @icon-park/vue-next / 内联 SVG） |
| 布局 | Layout / Menu / Grid / Space / Divider |
| 表单 | Form + Input / Select / DatePicker 等 |
| 列表 | Table + Pagination + 顶部筛选 |
| 反馈 | Message / Notification / Modal / Drawer |
| 明暗 | body 主题属性 + CSS 变量；见 theme-mode.md |

## 工作流

### A. 生成可运行界面代码

1. 读 product-shell.md（锁壳）+ **sider-collapse.md（侧栏折叠）** + component-source.md（实现层组件用法）
2. **写 Page Spec**（page-spec.md）：类型、模块、接口占位、反馈文案来源
3. 读 style-usage.md + design-tokens.md；明暗则 theme-mode.md
4. 读 layout-patterns.md；工作台再读 content-ux.md
5. **套模板** `references/templates/`（workbench / list / detail）再改业务
6. 按需：modal / message / progress / drawer / ops / async-states
7. 输出：深蓝 Header + 白 Sider + 业务区 + loading/empty/error
8. 真实组件 props + IconPark；services 占位保留
9. **delivery-gates.md 全过** 后再交付

### B. 生成 UI 示意图

1. 5～10 行信息架构（可当简化 Page Spec）
2. image-prompts + 顶栏 #004EA2 + 浅蓝选中；控件符合 style-usage
3. 出图并验收壳与密度；**规则文字优先于旧参考图**

### C. 评审

1. 壳色与菜单选中
2. delivery-gates + style-usage
3. components / 异步反馈
4. 问题列表 + 改法


## 按需加载

| 场景 | 读取 |
|------|------|
| 顶栏/菜单/参考图 | product-shell.md、examples/ |
| **样式原则与组件用法** | style-usage.md |
| 色板/字号/间距 | design-tokens.md |
| 组件 API | components.md |
| **组件视觉规格（Avatar/Tag/…）** | component-visual.md |
| 页面骨架 | layout-patterns.md |
| 出图 | image-prompts.md |
| 禁止项 | do-dont.md |
| 明暗 | theme-mode.md |
| 工作台内容区 | content-ux.md |
| **弹窗 Modal（确认/表单/确认+输入）** | modal-patterns.md |
| **全局提示 Message（删除成功等）** | message-patterns.md |
| **进度条 Progress** | progress-patterns.md |
| **实现层组件权威** | component-source.md |
| **页面契约 Page Spec** | page-spec.md |
| **异步/接口占位** | async-states.md |
| **交付闸门** | delivery-gates.md |
| **代码模板** | templates/workbench.md、list.md、detail.md |
| **抽屉 Drawer** | drawer-patterns.md |
| **批量/空态/权限** | ops-patterns.md |
| **侧栏折叠** | sider-collapse.md |


## 交付要求

**必须先通过** [references/delivery-gates.md](references/delivery-gates.md)。
含侧栏页额外过 [references/sider-collapse.md](references/sider-collapse.md) 第 5 节自检。


- 明暗：需要时顶栏切换 + 变量色；顶栏保持 #004EA2
- 代码可运行；包名仅技术说明
- 默认带齐壳层；正文 14、间距 4 倍数、每焦点区一个 primary
- 中文文案；空状态明确；按钮动词一致
- **界面不出现规范注释 / token 名 / 实现库品牌名**
- 进度用 Progress 组件；操作结果用 Message；弹窗顶栏使用指定渐变
- 组件用法与实现层一致，仅壳/弹窗顶栏/页面结构按本 skill 定制
- 业务可改按钮 primary，顶栏默认仍 #004EA2，菜单选中仍浅蓝（除非用户改壳）
