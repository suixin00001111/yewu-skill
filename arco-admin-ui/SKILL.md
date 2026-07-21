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
  实现层默认技术栈倾向 @arco-design/web-react；若用户指定 Vue 则用 @arco-design/web-vue。
---

# ui组业务组件

按 **ui组业务组件** 规范产出中后台界面与 UI 图。产品壳以参考图为准：
**深蓝通栏顶栏 + 浅色侧栏浅蓝选中菜单**。禁止套用 Ant Design 默认壳、Element 绿、
通用 AI 紫渐变/玻璃拟态等非本规范审美。

## 硬约束

- **顶栏必须**：通栏深蓝 `#004EA2`，白字系统名（按业务填写，勿固定示例名）+ 右侧工具区（见 [references/product-shell.md](references/product-shell.md)）
- **菜单必须**：白底侧栏，选中项浅蓝底 `#E6F1FF` + 蓝色图标/文字（见 product-shell.md）
- 主色按钮/链接默认 `#165DFF`（`primary-6`）；语义色/中性色见 [references/design-tokens.md](references/design-tokens.md)
- 组件 API、命名、尺寸阶梯（mini/small/default/large）见 [references/components.md](references/components.md)
- 中后台布局默认：**顶栏(深蓝) + 侧栏(白) + 内容区(浅灰底白卡片)**，见 [references/layout-patterns.md](references/layout-patterns.md)
- 工作台 / 表格列表 / **详情页**气质 **参考** examples 截图（结构可随业务变，壳层不可变；详情见 product-shell B3）
- 出图前套用 [references/image-prompts.md](references/image-prompts.md)
- 禁止项见 [references/do-dont.md](references/do-dont.md)
- 代码优先真实组件库组件，不要手写“长得像规范”的裸 HTML 冒充

## 技术栈默认（实现层，勿改包名）

| 项 | 默认 |
|----|------|
| React | `@arco-design/web-react` + `@arco-design/web-react/dist/css/arco.css` |
| Vue | `@arco-design/web-vue`（仅用户指定 Vue 时） |
| 图标 | `@arco-design/web-react/icon` |
| 布局 | `Layout` / `Menu` / `Grid` / `Space` / `Divider` |
| 表单数据 | `Form` + `Input` / `Select` / `DatePicker` 等 |
| 列表 | `Table` + `Pagination` + 顶部筛选区 |
| 反馈 | `Message` / `Notification` / `Modal` / `Drawer` |

组件库文档（实现参考）：https://arco.design/

## 工作流

### A. 生成可运行界面代码

1. 读 **product-shell.md**（锁死顶栏/菜单壳）
2. 读 design-tokens.md（色板/字号/圆角）
3. 读 layout-patterns.md，选定页面类型（工作台 / 列表 / 表单…）
4. 读 components.md，按页面区块选型组件
5. 输出结构：
   - 全局壳：深蓝 Header + 白 Sider Menu + Content
   - 业务区（工作台卡片 / 筛选+表格 / 表单 / 详情分块+Descriptions / 可选步骤条）
   - 关键态（loading / empty / error）
6. 使用真实组件名与 props；尺寸用 `size="small"` 等官方枚举
7. 用 product-shell 自检清单 + do-dont.md 后再交付

### B. 生成 UI 示意图（位图 mockup）

1. 先写 5～10 行信息架构（导航、主操作、表格列、筛选字段）
2. 读 image-prompts.md，**强制写入顶栏 #004EA2 与浅蓝选中菜单**
3. 调用 imagegen（或用户指定出图方式）
4. 对照验收：顶栏色、菜单选中态、是否误做成深色侧栏

### C. 评审现有界面

1. 壳层：顶栏是否 #004EA2 通栏、菜单是否白底+浅蓝选中
2. 对照 tokens：主色、文字四级色、边框/填充
3. 对照 components：按钮层级、表格密度、表单标签布局
4. 输出：问题列表 + 按本规范的改法（组件级）

## 按需加载

| 场景 | 读取 |
|------|------|
| 顶栏/菜单/工作台/列表参考 | references/product-shell.md |
| 参考截图 | references/examples/（workbench、table-list、detail-with-steps、detail-plain） |
| 颜色/字体/间距/圆角 | references/design-tokens.md |
| 组件选型与用法 | references/components.md |
| 页面骨架/断点 | references/layout-patterns.md |
| 出 UI 图 | references/image-prompts.md |
| 禁止与反例 | references/do-dont.md |

## 交付要求

- **代码**：可粘贴运行的页面/组件示例；写明依赖包名（实现层包名保持不变）
- **壳层**：默认带齐深蓝顶栏 + 白侧栏菜单，除非用户只要内容区片段
- **UI 图**：注明分辨率（桌面 1440×900 或 1920×1080）、页面名称、主操作
- **文案**：中文优先，避免乱码占位；空状态给明确说明
- 若用户给了业务品牌主色，可覆盖按钮 `primary`，但 **顶栏仍默认 #004EA2**，菜单选中仍为浅蓝底（除非用户明确改壳）
