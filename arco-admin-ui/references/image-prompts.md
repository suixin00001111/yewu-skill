# ui组业务组件 · UI 出图 Prompt 模板

用于 imagegen / 外部文生图。每次出图必须包含「固定约束后缀」，并体现产品壳。

对照：`examples/workbench.png`、`examples/table-list.png`。

## 1. 提示词结构

```
[产品] 中后台管理界面高保真 UI mockup，
[页面类型：工作台/列表/表单/详情/仪表盘]，
布局：顶部通栏深蓝 Header 背景 #004EA2 白字系统名与右侧工具，
左侧白色侧栏菜单，当前项浅蓝底 #E6F1FF 蓝色图标文字，
内容区浅灰底 #F2F3F5 白卡片，
内容：[用中文描述真实模块名、筛选项、表格列、按钮]，
视觉：ui组业务组件风格，主按钮 #165DFF，
文字色 #1D2129 / #4E5969 / #86909C，
边框 #E5E6EB，小圆角 2–4px 控件、卡片约 4–8px，
14px 级中文 UI，企业级 SaaS Admin，干净克制，
[分辨率，如 desktop 1920x1080]，
[固定约束后缀]
```

## 2. 固定约束后缀（每次追加）

```
enterprise admin UI, full-width header bar color #004EA2 white product title (use real product name, not a fixed demo name),
white left sidebar, selected menu item background #E6F1FF blue text,
primary buttons #165DFF, content background #F2F3F5 white cards,
NOT dark sidebar, NOT white top header, NOT Ant Design shell,
NOT Material purple, NOT Element UI, no glassmorphism, no neon gradients,
no heavy shadows, no oversized rounded corners, no decorative 3D,
clean Chinese enterprise dashboard, sharp readable text,
high-fidelity flat UI screenshot style, no watermark
```

## 3. 页面速用模板

### 列表页

```
组织列表页，顶栏深蓝 #004EA2 写系统名称（按业务，勿写死示例），
左侧白菜单选中「工作台」浅蓝底，内容面包屑「综合管理 / 便函管理」，
分段「组织列表 / 组织架构图」，筛选组织名称与编码，查询主按钮 #165DFF 与重置，
右上「+ 添加」，表格含序号、树形组织名称、编码、类型点、操作（新增下级/编辑/排序/删除红字），
底部分页，ui组业务组件风格...
```

### 工作台

```
工作台，顶栏 #004EA2，左白菜单浅蓝选中，内容区浅灰底左右两栏：
左栏待办浅蓝横幅卡含主按钮与四步节点、指标砖网格与折线图、业务动态与内嵌浅灰表头表格，
右栏对象上下文卡状态Tag、常用功能彩色IconPark宫格、变更月历与事件列表，
主色#165DFF，无抄白顶栏/青绿主题，无营销英雄区无玻璃拟态，
ui组业务组件风格...
```

### 详情页（有步骤）

```
详情页，顶栏 #004EA2，左白菜单浅蓝选中，
内容标题「计划信息」右上返回，下方水平 Steps 风格步骤条（圆点节点+连接线，当前主色，非箭头色块），
以下基本信息/计划信息/专家组分块，蓝竖条标题，Descriptions 浅灰标签多列，
下方专家表格，ui组业务组件风格...
```

### 详情页（无步骤）

```
详情页，顶栏 #004EA2，无流程步骤条，
标题与返回/保存在顶部白底背景条内，下方每个模块单独白卡片分块与表格，ui组业务组件风格...
```

## 4. 出图后自检

- [ ] 顶栏是否为通栏 `#004EA2`（而非按钮蓝或白顶栏）
- [ ] 侧栏是否白底 + 选中 `#E6F1FF`（而非深色侧栏）
- [ ] 主按钮是否接近 `#165DFF`
- [ ] 是否小圆角、浅灰底+白卡片
- [ ] 中文是否可读、无乱码条
- [ ] 无紫粉渐变、无玻璃拟态
- [ ] 保存/返回是否在页面顶部白底/浅灰条内而非表格内？
- [ ] 步骤条是否为节点+连接线（非箭头块）？
- [ ] 表格是否浅灰表头？
- [ ] 详情页是否有蓝竖条分块 + Descriptions？有流程时是否有蓝/灰步骤条？

## 5. 明暗出图

- 默认出 **Light**。
- 用户要暗色稿时：内容区组件库暗色气质，**顶栏仍 #004EA2 白字**，侧栏深色表面 + 主色选中，无霓虹玻璃。
- 顶栏右侧可画半月亮/太阳切换图标。
