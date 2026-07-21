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
- 右侧：消息/举报角标、通知、设置、头像等，图标与文字为白/浅色
- 顶栏与下方区域无圆角大卡片包壳；就是一条实色条

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
- 宽度展开约 **200–220px**，可折叠
- 菜单项：左图标 + 文案；可分组、可展开（`SubMenu`）
- **选中态（确定）**：
  - 背景：`#E6F1FF`（浅蓝底）
  - 文字/图标：主色系蓝（约 `#165DFF` / `#398EFF`）
  - 圆角小（约 4px），左右留边，不要整条粗色条贴边的 Ant 风
- 未选中：文字 `#1D2129` / `#4E5969`，白底，hover 浅灰填充
- 当前模块选中后整行浅蓝底

实现提示：

```tsx
<Sider width={220} style={{ background: '#fff', borderRight: '1px solid var(--color-border-2)' }}>
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

### B1. 工作台（对照 workbench.png）

推荐区块：快捷操作、我的任务、待办事项、常用系统、通知公告、右侧用户卡/日程。  
气质：多白卡片、浅灰底、信息密度中等、卡片圆角小（4–8px）。

### B2. 表格/列表页（对照 table-list.png）

面包屑 → 可选分段 → 筛选 → 工具栏主操作 → Table → Pagination。  
操作列：蓝字链接 + 红色删除。

### B3. 详情页（对照 detail-with-steps / detail-plain）

详情页内容区气质 **默认按下列两种模板**（业务字段可变；壳层仍走 A 节）。

#### 共同结构（两种详情都有）

1. **页头行**：左侧大标题（如「质量管理量化评价计划信息」），右侧次按钮「返回」
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
- 当前步：实色主蓝底/箭头块（`#165DFF`），白字 + 图标
- 未完成步：灰底（`#E5E6EB` / `#C9CDD4` 一带），深灰字
- 步骤之间用箭头/chevron 衔接（流程条形态，而非仅圆点 Steps）
- 典型：量化评价计划 → 书面评价&现场评价 → 结果汇总 → 问题项详情

实现可用 `Steps` 自定义节点，或一排可点击的流程块：

```tsx
{/* 页头 */}
<div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 16 }}>
  <Typography.Title heading={5} style={{ margin: 0 }}>质量管理量化评价计划信息</Typography.Title>
  <Button>返回</Button>
</div>
{/* 流程步骤条 */}
<div style={{ display: 'flex', marginBottom: 20 }}>
  {['量化评价计划', '书面评价&现场评价', '结果汇总', '问题项详情'].map((t, i) => (
    <div
      key={t}
      style={{
        flex: 1,
        height: 48,
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        background: i === 0 ? '#165DFF' : '#E5E6EB',
        color: i === 0 ? '#fff' : '#4E5969',
        clipPath: i === 0
          ? 'polygon(0 0, calc(100% - 12px) 0, 100% 50%, calc(100% - 12px) 100%, 0 100%)'
          : undefined,
      }}
    >
      {t}
    </div>
  ))}
</div>
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

### B4. 允许变化的部分

- 业务字段、列、菜单文案、卡片模块可按需求改
- **不可变**：深蓝顶栏 `#004EA2`、浅色侧栏 + 浅蓝选中菜单、主色按钮 `#165DFF`、浅灰底+白卡片
- 详情页 **推荐** 蓝竖条分块 + Descriptions 边框表格式（可按字段多少调列数）

---

## C. 生成自检清单

- [ ] 顶栏是否为通栏 `#004EA2` 白字？
- [ ] 侧栏是否白底，选中是否浅蓝底 `#E6F1FF`？
- [ ] 是否没有做成深色侧栏 / 白顶栏反转壳？
- [ ] 列表页是否具备筛选 + 主按钮 + 表格 + 分页闭环？
- [ ] 工作台是否以卡片分区而非大营销英雄区？
- [ ] 详情页是否：标题+返回、蓝竖条分块、Descriptions 标签浅灰底？
- [ ] 若有流程：步骤条当前步主蓝、未完成灰；若无流程：不要硬加步骤条？
