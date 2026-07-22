# ui组业务组件 · 产品壳与参考页（必读）

本文件固化 **确定样式** 与 **参考样式**。生成任何中后台页前先读本文件。

参考截图（风格对照，非像素级还原）：
- [examples/workbench.png](examples/workbench.png) — 工作台
- [examples/table-list.png](examples/table-list.png) — 组织/表格列表页
- [examples/detail-with-steps.png](examples/detail-with-steps.png) — 详情页（有步骤/进度条）
- [examples/detail-plain.png](examples/detail-plain.png) — 详情页（无步骤条）

---

## A. 确定样式（必须遵守，不可改成别的壳）

### A1. 顶栏 Header（必须）

- **通栏深蓝顶栏**，横跨整个视口宽度（在侧栏之上，不是嵌在内容区里）
- 背景色固定：`#004EA2`（不要用白顶栏、灰顶栏、渐变顶栏）
- 高度约 **48–56px**
- 左侧：系统/产品名称（**业务占位，按用户产品名填写**，截图中的名称仅举例），**白色**字，字重 500–600
- 右侧：消息/举报角标、通知、**明暗切换**、设置、头像等，图标与文字为白/浅色（明暗细则见 [theme-mode.md](theme-mode.md)）
- 顶栏与下方区域无圆角大卡片包壳；就是一条实色条
- **明暗切换**：放在顶栏右侧工具区；实现用 body 主题属性切换暗色（实现层约定，见 theme-mode）；**顶栏色两态仍为 `#004EA2`**；侧栏/内容随主题变量变化，见 theme-mode.md

实现提示（React）：

```tsx
<Layout style={{ height: '100vh' }}>
  <Header
    style={{
      height: 52,
      lineHeight: '52px',
      background: '#004EA2',
      color: '#fff',
      padding: '0 20px',
      display: 'flex',
      alignItems: 'center',
      justifyContent: 'space-between',
    }}
  >
    <div style={{ fontSize: 16, fontWeight: 600 }}>{/* 系统名称：按业务填写，勿写死示例文案 */}系统名称</div>
    <Space size="large">{/* 举报 / 铃铛 / 设置 / Avatar */}</Space>
  </Header>
  <Layout>
    <Sider>...</Sider>
    <Content>...</Content>
  </Layout>
</Layout>
```

### A2. 左侧菜单 Menu（必须）

- **浅色侧栏**：背景白 `#FFFFFF`，不要深色侧栏
- 宽度展开 **220px**，收起 **48px**（仅图标）；**默认开启 collapsible**；折叠规范见 [sider-collapse.md](sider-collapse.md)（生成壳层必读）
- **禁止**侧栏与内容之间出现独立灰滚动轨/双 trigger/误开 resize 拖拽条
- 菜单项：左图标 + 文案；可分组、可展开（`SubMenu`）
- **选中态（确定）**：
  - 背景：`#E6F1FF`（浅蓝底）
  - 文字/图标：主色系蓝（约 `#165DFF` / `#398EFF`）
  - 圆角小（约 4px），左右留边，不要整条粗色条贴边的 Ant 风
- 未选中：文字 `#1D2129` / `#4E5969`，白底，hover 浅灰填充
- 当前模块选中后整行浅蓝底

实现提示：

```tsx
<Sider width={220} collapsible collapsedWidth={48} theme="light" style={{ background: '#fff', borderRight: '1px solid var(--color-border-2)' }}>
  <Menu selectedKeys={['workbench']} style={{ border: 'none', padding: '8px' }}>
    <Menu.Item key="workbench" style={{ background: '#E6F1FF', color: '#165DFF', borderRadius: 4 }}>
      工作台
    </Menu.Item>
  </Menu>
</Sider>
```

### A3. 内容区骨架（确定）

- 内容底：`#F2F3F5` / `#F7F8FA`
- 主业务落在 **白卡片** 上
- 顶栏下第一行：**面包屑**（可带折叠侧栏按钮）
- 主按钮/强调操作：`#165DFF`（`type="primary"`）
- 危险操作文字：红（删除）
- 行内操作：蓝色文字链 / `Button type="text"`

---

## B. 参考样式（大体按此气质，允许业务差异）

### B1. 工作台（对照 workbench.png + workbench-ops.png）

**壳不变**；内容区对标优秀业务工作台密度。详细模块见 [content-ux.md](content-ux.md)。

- 参考：`examples/workbench.png`（基础分块）、`examples/workbench-ops.png`（运营向：待办横幅、指标砖+趋势、右栏上下文/常用功能/日历、内嵌表——**只学内容区，不学其顶栏色**）
- 布局：左主栏（横幅/数据概览/动态/表格）+ 右辅栏（上下文卡、常用功能、日历）
- 可选内容区顶：业务 Tab + 搜索
- 卡片小圆角、浅灰底多白卡；指标两层表面；主按钮克制
- 图标：IconPark；禁止营销英雄区与抄参考图（**冲突时以 SKILL/layout 文字硬约束为准，图可能滞后**）换壳

### B2. 表格/列表页（对照 table-list.png）

面包屑 → 可选分段 → 筛选 → 工具栏主操作 → Table → Pagination。  
操作列：蓝字链接 + 红色删除。

### B3. 详情页（对照 detail-with-steps / detail-plain）

详情页内容区气质 **默认按下列两种模板**（业务字段可变；壳层仍走 A 节）。

#### 共同结构（两种详情都有）

1. **页头行（页面级操作）**：左侧大标题；右侧 **返回 / 保存 / 提交** 等整页按钮（全部在最上方，不进表格）
2. **分块信息区**：多个区块，每块标题左侧有 **蓝色竖条** + 标题文案（如「基本信息」「量化评价计划信息」「组建专家组」）
3. **描述列表（Descriptions）**：
   - 标签列浅灰底（约 `#F2F3F5` / `#F7F8FA`）
   - 值列白底
   - 多列栅格（常见 3 列：标签+值 成对横向排布；长文本可整行）
   - 有边框表格感，不是纯文本堆叠
4. **区块内可再嵌 Table**（如专家列表：序号、专家、依据、评价域、评分方式）
5. 白卡片承载，内容区浅灰底；无大营销头图

#### 变体 1：有步骤/进度条（detail-with-steps.png）

- 页头下方、第一信息块 **之上**，放 **横向步骤条（Steps）**
- 当前步：实现层 Steps 的 process 态（主色节点+文案），**非**大块箭头色条
- 未完成步：Steps wait 态（灰节点+灰字）
- 步骤之间用 **连接线** 衔接（组件库 Steps 默认形态，不要 chevron 色块条）
- 典型：量化评价计划 → 书面评价&现场评价 → 结果汇总 → 问题项详情

实现用组件库 `Steps`（节点 + 连接线）：

```tsx
{/* 页头：标题 + 操作，有背景容器 */}
<div
  style={{
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 16,
    padding: '12px 16px',
    background: '#FFFFFF',
    borderRadius: 4,
    border: '1px solid var(--color-border-2)',
  }}
>
  <Typography.Title heading={5} style={{ margin: 0 }}>计划信息</Typography.Title>
  <Space>
    <Button>返回</Button>
    <Button type="primary">保存</Button>
  </Space>
</div>
{/* 流程步骤条：禁止箭头色块 */}
import { Steps } from '@/components'; // 实现层组件库入口（以项目为准）
<Steps current={0} style={{ marginBottom: 16, background: '#fff', padding: '16px 20px', borderRadius: 4 }}>
  <Steps.Step title="量化评价计划" />
  <Steps.Step title="书面评价&现场评价" />
  <Steps.Step title="结果汇总" />
  <Steps.Step title="问题项详情" />
</Steps>
{/* 下列 Descriptions 分块 */}
```
#### 变体 2：无步骤条（detail-plain.png）

- **不要**流程 Steps
- 页头（标题 + 返回）后直接进入「| 基本信息」等 Descriptions 分块 + 表格
- 其余排版与变体 1 相同

#### 区块标题样式（确定气质）

- 左侧 **4px 宽蓝色竖条**（`#165DFF`）+ 右侧标题字 14–16、字重 500–600
- 标题与内容区间距约 12–16px

```tsx
<div style={{ display: 'flex', alignItems: 'center', gap: 8, marginBottom: 12 }}>
  <span style={{ width: 3, height: 14, background: '#165DFF', borderRadius: 1 }} />
  <span style={{ fontWeight: 600 }}>基本信息</span>
</div>
<Descriptions
  border
  column={3}
  data={[
    { label: '被评价单位', value: '北京汇智' },
    { label: '申请日期', value: '2023-10-10' },
    // ...
  ]}
/>
```

#### 何时用哪种变体

| 场景 | 选用 |
|------|------|
| 多阶段流程详情/办理过程 | **有步骤条** |
| 普通只读详情、查看档案 | **无步骤条** |
| 用户未说明 | 普通详情默认 **无步骤条**；文案含「流程/阶段/步骤/向导」时用有步骤条 |

### B5. 页面级操作栏与模块分块（确定逻辑）

适用于 **表单页 / 详情编辑页 / 新建编辑 / 配置页** 等带「保存、返回、提交、取消」的页面。

#### 页面级操作（必须在最上方）

- **保存、返回、提交、取消、发布** 等 **针对整页** 的按钮，一律放在 **内容区顶部页头行**（标题同一行右侧，或标题下一行工具条）。
- **不要** 放进表格操作列，**不要** 散落在每个模块底部，**不要** 塞进某一条表格行。
- 典型页头：左标题，右「返回 / 取消 / 保存」；**整行放在有背景色的白/浅灰容器内**。
- 主操作（保存/提交）用 `type="primary"`；返回/取消用 default。
- 长表单若需要吸底操作条，仅作补充；**顶部页头操作栏仍必须保留**（至少「返回」在顶部可见）。

```tsx
<div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: 16 }}>
  <Typography.Title heading={5} style={{ margin: 0 }}>计划信息编辑</Typography.Title>
  <Space>
    <Button>返回</Button>
    <Button>取消</Button>
    <Button type="primary">保存</Button>
  </Space>
</div>
```

#### 模块分块（每一模块单独一块）

- 页面按业务拆成 **多个独立模块块**，每块：
  - 独立白底区域（独立 `Card` 或带边框分块），块间距 12–16px
  - 块标题：左侧蓝竖条 + 模块名
  - 块内只放本模块字段 / 本模块子表
- **禁止** 把所有字段揉进一个无标题的大卡片里不分块。
- 示例：基本信息 | 计划信息 | 组建专家组 | 附件 —— 各占一块。


#### 步骤条 / 进度条样式（视觉对齐实现组件库 Steps，不强调品牌名）

- 使用实现层 `Steps` 组件默认形态（水平步骤：圆点/序号节点 + 连接线 + 文案），**不要**做成大块箭头流程条、chevron 色块条。
- 状态色：
  - 进行中 process：主色 `#165DFF` 节点 + 主色字
  - 已完成 finish：主色勾选/实心节点
  - 等待 wait：灰节点 `#C9CDD4` / 灰字 `#86909C`
- 节点尺寸与字号跟随组件库 small/default，中后台优先 `size="small"` 或 default。
- 放在页头操作栏下方、第一模块块上方；有流程才出现。

```tsx
import { Steps } from '@/components'; // 实现层组件库入口（以项目为准）
// 名义上对外称 ui组业务组件步骤条；实现用 Steps API
<Steps current={1} style={{ marginBottom: 16, background: '#fff', padding: '16px 20px', borderRadius: 4 }}>
  <Steps.Step title="量化评价计划" />
  <Steps.Step title="书面评价&现场评价" />
  <Steps.Step title="结果汇总" />
  <Steps.Step title="问题项详情" />
</Steps>
```

#### 表格样式（视觉对齐实现组件库 Table）

- 表头：浅灰底 `color-fill-2` / `#F2F3F5` 或组件默认表头，**不要**高饱和浅蓝整表头（避免非规范定制表头色）。
- 字号 14、行高紧凑；中后台可用 `size="small"` 或 default。
- 边框：默认表格线 `color-border-2`；斑马纹仅在需要时开启。
- 操作列（仅列表页行级）：`Button type="text"`，危险删除红色字。
- 分页：`Pagination` 挂在表格底部，风格跟组件库默认。

#### 页头标题 + 操作按钮区背景（确定）

- 页面标题与 **返回/保存/取消** 所在行，必须有 **独立背景容器**（白底或浅灰 `#F7F8FA` 卡片条），不是裸贴在内容灰底上。
- 建议：白底条 + 内边距 12–16px + 小圆角 4px + 可选浅边框；标题左、按钮右同一容器内。
- 该背景条在步骤条、模块块 **之上**。

```tsx
<div
  style={{
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 16,
    padding: '12px 16px',
    background: '#FFFFFF',
    borderRadius: 4,
    border: '1px solid var(--color-border-2)',
  }}
>
  <Typography.Title heading={5} style={{ margin: 0 }}>计划信息编辑</Typography.Title>
  <Space>
    <Button>返回</Button>
    <Button>取消</Button>
    <Button type="primary">保存</Button>
  </Space>
</div>
```

#### 和表格行操作的区分

| 类型 | 放哪里 | 例子 |
|------|--------|------|
| 页面级操作 | 仅顶部页头 | 保存、返回、提交、取消 |
| 模块内数据 | 对应模块块内 | Descriptions、模块内 Table |
| 列表页行操作 | 仅列表页表格操作列 | 编辑、删除、新增下级 |
| 表单/详情编辑页 | 不要用表格列承载整页保存/返回 | — |


### B4. 允许变化的部分

- 业务字段、列、菜单文案、卡片模块可按需求改
- **不可变**：深蓝顶栏 `#004EA2`、浅色侧栏 + 浅蓝选中菜单、主色按钮 `#165DFF`、浅灰底+白卡片
- 详情页 **推荐** 蓝竖条分块 + Descriptions 边框表格式（可按字段多少调列数）

---

生成界面时 **不要** 把本文件中的规则写成页面上的提示条/注释文案。

## C. 生成自检清单

- [ ] 顶栏是否为通栏 `#004EA2` 白字？
- [ ] 侧栏是否白底，选中是否浅蓝底 `#E6F1FF`？
- [ ] 侧栏是否可折叠（220↔48 仅图标），无中间灰轨、无双折叠条？（sider-collapse）
- [ ] 是否没有做成深色侧栏 / 白顶栏反转壳？
- [ ] 列表页是否具备筛选 + 主按钮 + 表格 + 分页闭环？
- [ ] 工作台是否以卡片分区而非大营销英雄区？
- [ ] 详情页是否：标题+返回、蓝竖条分块、Descriptions 标签浅灰底？
- [ ] 保存/返回等页面级按钮是否在顶部页头（不在表格里）？
- [ ] 页头标题+按钮是否有白/浅灰背景容器？
- [ ] 步骤条是否为节点+连接线（非箭头色块条）？
- [ ] 表格是否为浅灰表头的组件库 Table 气质？
- [ ] 是否每个业务模块单独一块 Card/分块？
- [ ] 若有流程：步骤条当前步主蓝、未完成灰；若无流程：不要硬加步骤条？

### 明暗主题自检（若启用）

- [ ] 顶栏有切换入口且为白/浅色图标
- [ ] body 主题属性切换后，实现层组件整体进入暗色
- [ ] 顶栏仍为 `#004EA2`
- [ ] Dark 下未写死 Light 的菜单选中 `#E6F1FF`
- [ ] 主题写入 localStorage 并可恢复
- [ ] 工作台内容是否具备概览/待办横幅或指标砖（对标 workbench-ops 密度，且未改壳色）？

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


## Steps 步骤条宽度（强制）

- 步骤条 **充满** 所在白色容器/模块内容区宽度（`width: 100%`）。
- **保留** 模块内边距 16–20px，不贴卡片边。
- 节点固定；**连接线 flex:1** 均分拉满；禁止挤在容器左侧一截。
- 形态：节点 + 连接线；禁止箭头色块条。

## 侧栏折叠（强制）

完整规范与禁止坏例见 [sider-collapse.md](sider-collapse.md)。壳层生成必须包含可折叠 Sider，且通过该文自检。
