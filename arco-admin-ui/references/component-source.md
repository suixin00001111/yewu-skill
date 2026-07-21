# ui组业务组件 · 实现层与组件用法权威

## 1. 关系说明

本 skill（**ui组业务组件**）分两层：

| 层级 | 内容 | 以谁为准 |
|------|------|----------|
| **产品壳与业务版式** | 顶栏色、侧栏选中、列表/工作台/详情结构、弹窗顶栏渐变、页边距等 | **本 skill 硬约束** |
| **单组件视觉、状态、API、示例代码** | Button / Table / Form / Progress / Message / Modal 等 | **实现层中后台组件库**（与本 skill 配套的工程组件用法） |

- **界面文案 / 示意图 / 说明文档**：禁止出现第三方设计系统或厂商品牌名。  
- **代码 import / package.json**：使用项目既定实现层依赖包名即可（保证可安装运行），**不要**把品牌名写进页面或对外文档。

## 2. 技术栈（实现依赖，勿写入 UI）

| 场景 | 约定 |
|------|------|
| React（默认） | 项目实现层 React 组件库 + 其全局样式 |
| Vue（仅用户指定） | 项目实现层 Vue 组件库 + 其全局样式 |
| 图标 | IconPark（outline 优先） |

生成代码时：

1. **优先真实组件**，按实现层官方 props（`type` / `status` / `size` / `loading` 等）。  
2. 用 CSS 变量 / 主题变量覆盖业务色；**不要**手写一套假组件皮肤。  
3. 需要定制时：`className`、全局 ConfigProvider、CSS 变量；弹窗顶栏等用本 skill 指定样式覆盖。

## 3. 开发时如何查组件

以**项目内组件文档 / 实现层组件索引**为准，按组件名查阅：

- Layout / Menu / Table / Form / Modal / Message / Progress / Steps …
- 暗色：见本 skill `theme-mode.md` + 实现层主题文档  
- 进度条细节：本 skill `progress-patterns.md`  
- 全局提示：本 skill `message-patterns.md`  
- 弹窗：本 skill `modal-patterns.md`

## 4. 定制覆盖清单（仅这些可偏离组件库默认皮肤）

1. Layout Header 背景 `#004EA2`、白字  
2. Menu 选中底 `#E6F1FF`、主色文字  
3. Modal 标题区背景 `linear-gradient(180deg, #eaf0ff 0%, #ffffff 34%)`  
4. 业务主按钮默认 `#165DFF`（与 primary 一致时可直接用 primary）  
5. 页面结构：工作台无页头标题、列表标题单独一行、详情页头操作条等  

其余 Table 描边、Progress 形态、Form 校验、Message 位置等 **按实现层组件默认行为**。

## 5. 模板与契约

- Page Spec：page-spec.md
- 骨架：templates/workbench.md、list.md、detail.md
- 闸门：delivery-gates.md
