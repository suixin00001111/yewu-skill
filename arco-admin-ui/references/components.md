# ui组业务组件 · 组件选型与用法

全局壳必须先符合 [product-shell.md](product-shell.md)。
样式原则与反馈层级见 [style-usage.md](style-usage.md)。
Token 见 [design-tokens.md](design-tokens.md)。
组件 API 权威见 [component-source.md](component-source.md)。
生成前 [page-spec.md](page-spec.md)；模板 [templates/](templates/)；异步 [async-states.md](async-states.md)。

实现层包名（保证可安装运行，勿写入界面文案）：
- React 默认：实现层 React 组件库 + 全局 CSS
- Vue（仅用户指定）：实现层 Vue 组件库


## 1. 页面级组件

| 需求 | 组件 | 要点 |
|------|------|------|
| 整体框架 | Layout / Header / Sider / Content | Header 48-60；**Sider 默认 collapsible 220↔48**，见 sider-collapse；顶栏色见 product-shell |
| 导航 | Menu | 垂直；选中浅蓝底 |
| 面包屑 | Breadcrumb | 二级及以上 |
| 页头 | Typography + Space | 左标题右操作；有背景容器 |
| 栅格/间距 | Grid / Row / Col / Space / Divider | gutter 16-20 |

## 2. 数据录入

Form / Form.Item / Input / TextArea / Search / Password / InputNumber / Select / Cascader / TreeSelect / DatePicker / TimePicker / RangePicker / Switch / Checkbox / Radio / Upload / Transfer / Rate / Slider

### Form 约定
- 筛选：inline 或栅格；查询 primary + 重置 default
- 编辑：vertical 或 horizontal（label 定宽）
- 校验文案简洁中文，贴近字段
- 提交 loading；长表按 Card 分模块；整页保存在顶部页头
- 同排 size 一致；密集页优先 small

## 3. 数据展示

Table / Pagination / Descriptions / Statistic / Tag / Avatar / Badge / Tree / Timeline / Steps / Empty / Result / Skeleton / Collapse / Card / Tabs / Image / Progress / Alert

### Table
- 主信息左、操作列 fixed right、text 按钮
- rowSelection + 顶栏批量；size=small；scroll.x
- 浅灰/默认表头；状态 Tag（色+文字）；数字列右对齐
- **描边**：详情明细表用 `border` + `borderCell`；列表主表至少 `border`
- 筛选在表上方独立区

## 4. 反馈

| 场景 | 组件 |
|------|------|
| 轻成功/失败/删除完成等 | **Message**（message-patterns） |
| 较长通知 | Notification |
| 删除确认 | Popconfirm / Modal.confirm |
| 复杂表单/详情 | Drawer 或整页 |
| 阻断 / 业务弹窗 | Modal（分 A/B/C，见 modal-patterns） |
| 页内提示 | Alert |
| 加载 | Spin / Skeleton |



## 侧栏折叠

默认可折叠侧栏，强制规范见 [sider-collapse.md](sider-collapse.md)。

## 批量 / 空态 / 抽屉

- 批量与空态、权限轻量：[ops-patterns.md](ops-patterns.md)
- 抽屉：[drawer-patterns.md](drawer-patterns.md)
- 异步状态与接口占位：[async-states.md](async-states.md)

## 进度条 Progress

百分比/任务进度用实现层 **Progress**，细则见 [progress-patterns.md](progress-patterns.md)。

| 场景 | 用法 |
|------|------|
| 指标完成率 | `type="line"` + `percent` + 常 `size="small"` |
| 完成/失败 | `status="success" | "error" | "warning"` |
| 仪表 | `type="circle"` |
| 业务阶段 | 用 **Steps**，不用 Progress |

当前进度主色、轨道中性；默认可 `showText`；全宽于容器内。

## 全局提示 Message（强制）

操作结果反馈必须使用全局 `Message`，规范见 [message-patterns.md](message-patterns.md)。

| 结果 | API | 文案示例 |
|------|-----|----------|
| 成功 | `Message.success` | 删除成功、保存成功、提交成功 |
| 失败 | `Message.error` | 删除失败，请重试 |
| 警示 | `Message.warning` | 部分数据未保存 |
| 说明 | `Message.info` | 已切换至… |
| 进行中 | `Message.loading` | 提交中…（结束后 success/error） |

**删除**：确认组件（Modal/Popconfirm）只负责决策；**完成后必须** `Message.success`/`error`，不要静默、不要成功 Modal。

默认顶部居中、约 3s 关闭、带类型图标；同屏控制条数。

## 弹窗 Modal（三类强制样式）

业务弹窗视觉与结构以 [modal-patterns.md](modal-patterns.md) 为准，生成/评审时必须区分：

**文案不固定**：标题、正文、表单项、按钮文字一律按用户/业务配置；文档中的「提示 / 添加子账号 / 取消订单」等仅为结构示例。

| 类型 | 场景 | 关键结构 | 底栏默认（可改） |
|------|------|----------|--------------|
| **A 纯确认** | 二次确认、无额外字段 | 标题「提示」等 + 语义图标(信息蓝) + 问句 | 取消 + **确认** |
| **B 表单弹窗** | 短表新建/编辑（字段 ≤ 8） | vertical Form；必填红 `*`；全宽控件；字段 helper | 取消 + **确认提交** |
| **C 确认+输入** | 确认且必填 1 个关键值 | 警告/信息图标 + 说明 + Input（可单位后缀）+ 灰字风险提示 | 取消 + **确定** |

### 通用壳（三种共用）

- 白底圆角 **8px** + 中层阴影；遮罩半透明
- 顶栏：背景 `linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%)` + 左标题 + 右关闭 ×
- 内容区内边距 **20–24px**
- 底栏 **右对齐**：**次按钮在左、主按钮在右**；同一底栏仅一个 primary（`#165DFF`）
- 宽度：确认类约 400–480；表单类约 480–560

### 选型分工

| 场景 | 选用 |
|------|------|
| 短确认 / 删除 | Modal A 或 Popconfirm |
| 短表新建编辑 | Modal B |
| 确认 + 金额/原因等单字段 | Modal C |
| 字段多 / 需对照页内上下文 | Drawer 或整页 |
| 已成功/失败轻反馈 | Message（勿再叠确认框） |

**禁止**：主按钮在左；无取消；无标题无关闭；长表硬塞窄 Modal；界面出现第三方库品牌名。

## 5. 按钮层级

| 层级 | 用法 | API |
|------|------|-----|
| 主操作 | 每焦点区唯一 CTA | type=primary |
| 次操作 | 取消/导出/重置 | default |
| 文本 | 行内 | type=text |
| 虚线 | 添加一项 | type=dashed |
| 危险 | 删除 | danger |

## 6. 图标（IconPark）

业务图标统一 IconPark outline；顶栏/菜单 16-18px；禁止 Emoji 主图标体系。
React 可用 @icon-park/react，按钮内 icon 使用 outline 尺寸 16。

## 7. 页面类型组合

| 页面 | 组合 |
|------|------|
| 列表 | 筛选 + 工具栏 + Table + Pagination |
| 新建/编辑 | 顶页头操作条 + 分模块 Card + Form（可 Steps） |
| 详情 | 顶页头 + 可选 Steps + Descriptions + 子表 |
| 工作台 | 见 content-ux |
| 设置 | Tabs/锚点 + Form |
| 登录 | 居中 Card + Form，无侧栏 |

## 8. 详情页

页头有背景条；Steps 仅多阶段；分块蓝竖条标题；Descriptions；子表 small。

## 9. 页面级操作（强制）

保存/返回/提交/取消只在最上方页头；不进表格操作列；不按模块重复保存（除非用户要求）。

## 10. 模块分块（强制）

一模块一 Card + 标题；间距 16-20。

## 11. 步骤条

Steps 节点+连线；禁止箭头色块条。

## 12. 页头背景条

白/浅灰容器，内边距 12-16，圆角 4。

## 13. 明暗

引入实现层全局 CSS；body 主题属性切换；顶栏切换 + localStorage（建议键 ui-theme）；业务色走 CSS 变量。

## 14. 最小骨架要点

Header 背景 #004EA2；Sider 白底 Menu；Content 使用 var(--color-fill-2)；筛选 Form + primary 查询；Table size=small；新建按钮使用 IconPark Plus。

## 15. 优先级

产品壳色 > style-usage 产品态原则 > 单组件习惯

## 模块内容区内边距（强制）

详情/表单分块 Card 内容区统一内边距 **16–20px**；Descriptions 与字段表不得贴边（禁止 body `padding:0` 全出血）。与内容区页边距 16–20 对齐。

## Avatar 与组件视觉

头像形状/尺寸/头像组/交互态及 Tag/Badge 等视觉细则见 [component-visual.md](component-visual.md)。顶栏头像建议 28–32px 圆形。

## Steps 步骤条宽度（强制）

- 步骤条必须 **充满其所在白色容器/模块内容区的宽度**（`width: 100%`）。
- **保留容器内边距**：由外层 Card / page 模块的 16–20px 内边距承担，步骤条不要再缩成左对齐一小截。
- 布局：节点 `flex: none`；**连接线 `flex: 1`** 均分拉满中间空隙；步骤之间视觉间距靠 line 的水平 margin（约 8–12px）。
- 实现层：`Steps` 放在全宽容器内；勿给 Steps 固定过窄 max-width；水平 `direction=horizontal` 铺满。
- 禁止：步骤条挤在容器左侧、中间大片空白；禁止为了拉满而取消卡片内边距贴边。

## 页面标题与页头操作（canonical）

### 按页面类型

| 页面类型 | 页面标题 | 主操作位置 |
|----------|----------|------------|
| **工作台** | **不要**单独页头标题名（不要「工作台」大标题条） | 内容区内业务按钮；无整页标题行 |
| **列表 / 普通业务页** | **需要**简洁页名（参考「产品列表」：16px / 字重 500，左对齐） | 标题**单独一行**；「新建」等放在**筛选下方、表格上方**工具行右对齐，**禁止与标题左右并列** |
| **详情 / 表单编辑** | **需要** | 页头条（有背景容器）：左标题 + 右返回/取消/保存；页面级操作仅在此条 |

### 列表页推荐结构

```
面包屑（可选）
┌─ 白卡片 ─────────────────────────────┐
│  页面标题（单独一行，无右侧并列按钮）   │
│  筛选区 + 查询 / 重置                  │
│  表格工具行（右：新建等）               │
│  Table + Pagination                    │
└────────────────────────────────────────┘
```

### 禁止

- 工作台再套「工作台」页头标题
- 列表页只有面包屑、没有页面名称
- 列表页标题与「新建」等主按钮同一行左右并列
- 把列表标题做成营销大标题 / 厚重双行英雄区


## Table 描边与尺寸（对齐实现层）

| 场景 | 要求 |
|------|------|
| 详情分块明细表 | 必须 `border` + `borderCell`（完整网格） |
| 列表主表 | 至少 `border`；需要列网格时加 `borderCell` |
| 表头 | 浅灰；禁止高饱和蓝表头 |
| padding | 默认约 **9px 16px**；`size="small"` 约 **5px 16px** |
| 边框色 | 中性，禁止主色描边表 |


## Steps 步骤条宽度（强制）

- 步骤条 **充满** 所在白色容器/模块内容区宽度（`width: 100%`）。
- **保留** 模块内边距 16–20px，不贴卡片边。
- 节点固定；**连接线 flex:1** 均分拉满；禁止挤在容器左侧一截。
- 形态：节点 + 连接线；禁止箭头色块条。

## 弹窗 Modal（样式强制）

业务弹窗视觉与三类模板（确认 / 表单 / 确认+输入）见 [modal-patterns.md](modal-patterns.md)。

| 要点 | 规则 |
|------|------|
| 顶栏 | `linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%)` + 标题 + 关闭 |
| 底栏 | 右对齐，取消左、主色确认右 |
| 确认 | 语义图标 + 文案 |
| 表单 | vertical、必填红星、控件满宽 |
| 确认+输入 | 图标说明 + Input（可带单位后缀）+ 风险灰字 |

